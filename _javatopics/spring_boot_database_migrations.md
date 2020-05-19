---
topic: "Spring Boot: Database Migrations"
desc: "When you need to make a change to your database schema for an app in progress"
category_prefix: "Spring Boot: "
indent: true
---
## Intro to Migrations
When you have a running app in production (say, on Heroku), you may need to make changes to the database as you continue to develop- adding a new table, a new column to an existing table, or changing a table name, for example. Sometimes you can rely on the default auto-ddl tools (which use your code and the framework conventions to determine the SQL commands probably needed to create new tables) incorporated into your framework to take care of that work behind the scenes, but sometimes this might not be sufficient. For example, if you change the name of an entity and expect the name of the corresponding data table to be changed, the auto-ddl tool may simply create an entirely new table. In Spring Boot, if you change the name of a column variable in an entity, the auto-ddl will likely just add a column with the new name and will not remove or replace the column with the old name. Often, there isn't a lot of good documentation on auto-ddl behavior, so it can also be hard to figure out the problem when something does go wrong.

Migrations are a way to explicitly manage database changes with SQL commands. They make the process of database management more transparent and provide a form of version control- once you successfully run a migration, you retain that SQL file as a record of the changes that you just made. If anything goes wrong with the database later on, you know you can safely roll back the database to an earlier version.

## Flyway Migrations with Maven in Spring Boot on Heroku
We will be using the Flyway tool for database migrations in Spring Boot apps. Here are some instructions and notes on setting up Flyway for your app, writing your first migrations, and setting up your Heroku environment to run those migrations for your production app. We'll be using Maven to trigger the migrations on Heroku. This tutorial also assumes you are using the Postgres add-on for your database on Heroku. The process should be similar with any SQL-based database management system, but the details may be slightly different.

Before going into the mechanics of the migrations themselves, it's important to know a few things about the Heroku release process. When you deploy your app (usually from a GitHub repo), Heroku must go through several phases before the app is ready to use on the internet. 

The first is the build phase, where any dependencies are fetched and your code is compiled into a "slug"- a bundle that is ready to run. In Spring Boot, this means running some equivalent to `mvn compile`. A new release of your app is created here.

If you don't specify otherwise, the next phase is usually the startup phase, which begins when you navigate to the app. 

In between build and startup, however, there is the **release** phase. If no actions are specified, then nothing happens here and the newest release of your app immediately becomes available. However, we are going to do some extra work here to make sure that all database migrations are triggered and run during release phase. The first reason for this is that, if we just rely on the default Flyway migration setup, the migrations will be run at *startup*. If you have a lot of migrations, or some large ones, the time it takes to run those migrations might exceed Heroku's boot-timeout, which usually crashes your app. Another reason is that, if you specify release-phase actions, then your latest app release doesn't become available until those actions successfully complete. That means that you will catch any problems with your migrations before the app is released, and you won't have to wait until someone actually tries to access your app to see if the migrations will cause it to crash.

We'll introduce the steps to run the migrations in release phase later on. You can read more about the Heroku build process here: <https://devcenter.heroku.com/articles/how-heroku-works#building-applications>

### Adding Flyway dependencies:
You can read the official Heroku documentation on setting up Flyway here: <https://devcenter.heroku.com/articles/running-database-migrations-for-java-apps#using-flyway>

We'll go over the steps in more detail here.
* You'll need to add `flyway-core` as a dependency and the `flyway-maven-plugin` as a plugin to your pom.xml:
```
    <dependency>
      <groupId>org.flywaydb</groupId>
      <artifactId>flyway-core</artifactId>
      <version>6.4.2</version>
    </dependency>
```
```
   <plugin>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-maven-plugin</artifactId>
    <configuration>
     <url>${env.JDBC_DATABASE_URL}</url>
    </configuration>
  </plugin>
```

* You'll then also need to add the Maven Wrapper to the project. This is a tool that allows projects to run `mvn` commands without having to install the full Maven build setup. We need it here because Maven is not available on Heroku after the build phase, and we need to run the migration commands in the release phase. You can read more about the Maven Wrapper at its source, here: <https://github.com/takari/maven-wrapper>. (In fact, you should check the README in that repo just in case they have updated the installation command, to make sure you get the most up-to-date version). You'll need to run the installation command in your terminal, when you are at the root of your project:
```
mvn -N io.takari:maven:0.7.7:wrapper
```

Then add the wrapper downloader files to your repo (they must be part of the source code that is updated to Heroku so the wrapper can be installed and the commands can be properly run):
```
git add mvnw .mvn
```

### Writing migrations

Naming conventions are very important with Flyway!

With the preliminary set up complete, you are now ready to write the migrations themselves. Migrations are written in pure SQL, the query language that is used to interact with most relational databases. If you've never written SQL before, you may want to take a moment to get familiar with useful commands like `CREATE TABLE...`, `SELECT.. FROM...`, and `INSERT INTO...`, among many others. Here is the first tutorial that came up when I searched "SQL tutorial"- perhaps it will be helpful: <https://www.w3schools.com/sql/>

With Spring Boot, your migrations *must* be in the directory `src/main/resources/db/migration/`. This is where Flyway will look for your migrations by default. There are ways to specify other paths, but we won't go into them for now.

Once you have created the `migration` directory and start writing migration files, you must follow the file-naming convention, which looks like this:
```
[PREFIX][VERSION]__[NAME].sql
```
* Prefix: indicates the "type" of migration. 
   * For our purposes, this will probably usually be `V`, for "version". That means each new round of migrations is bringing our database to a new version.
   * There is *no* space or underscore between the prefix and the version number
* Version: the version *number*. Usually in a format like `main_subpart`, like `3_0` for "version 3.0" or `2.1.1` for "version 2.1.1". You should can use *single* underscores or decimal points.
   * When you introduce several new migrations, they are ordered by their version numbers and executed in order. So something with version number `3.0` will always be executed before one with number `3.1`. This can be useful if you want to make a series of changes that where each change relies on some other change happening before it.
   * Usually you would collect a set of related changes under the same main version (`3.0` through `3.7`, for example), and the next big set of changes introduced later would be under another version.
* Name: a brief description of what the migration is doing
    * This is for your own records and version control so try to keep it simple and clear, like `add_customer_table` or `add_name_column_to_customer_table`.
    * The name is separated from the version number with a *double* underscore
Putting it all together, a typical migration file name would look something like:
```
V1_0__create_customer_table.sql
```
    
If you don't follow the naming convention precisely (inserting an underscore between prefix and version, or not using a double underscore to separarte the name), then the migration will fail to run.

The body of a migration file might look look something like this (just a SQL command):
```
CREATE TABLE IF NOT EXISTS customer(
    "id" bigint NOT NULL GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    "name" varchar(100),
    "email" varchar(100),
) 
```

## Running migrations on Heroku

Now that you have the dependencies, wrapper, and migrations in their proper places, all that remains to make sure these migrations are actually run on Heroku in the release phase. If you don't already have one, you will need to add a `Procfile` to the root of your project repo (at the same level as your pom.xml). The Procfile is a Heroku-specific file that lists actions for the app to execute at various stages. It is simply called `Procfile`- capital P and no file extension. You can read more about it here: <https://devcenter.heroku.com/articles/procfile>.

In the Procfile, we will use the following command to specify that we want the app to use the Maven Wrapper to run the flyway migrate command in the release phase, which will trigger the latest migrations:
```
release: ./mvnw flyway:migrate
```
If you have existing tables in your app's database at the time when you add your first migration, you might encounter a problem when you push this code to your repo and try to deploy. Flyway will detect that the schema (state of the database) it is starting from is not empty, and you will likely see an error like `Found non-empty schema(s) without schema history table! Use baseline() or set baselineOnMigrate to true to initialize the schema history table.` in your release logs before the release fails. 

This means that you need to indicate to Flyway that you want the current state of your database (with its existing tables) the be marked as the "baseline", so it can build on top of it. This blog was useful reading about this error: <https://dzone.com/articles/build-deploy-and-monitor-an-express-application-on>. 

The official way to resolve this is to add the line `spring.flyway.baseline-on-migrate: true` to your `applications.properties` (or `application.yml`) file. I had some problems with this approach, so I incorporated the command-line version of the baseline command into the release command. So, if you are still running into the baseline error even after adding that line to your `application.properties  ` and redeploying, you can try changing the release line in your Procfile to this:
```
release: ./mvnw flyway:baseline; ./mvnw flyway:migrate
```

This just runs two commands in sequence- first, set the Flyway baseline if it has not already been set, then run the Flyway migrations.

Once you've made these changes and you see in your release logs that your migrations are being successfully run, and your app deploys without issues, you may run into one final problem! If you try to navigate to your app, you may be met with the "Heroku Application Error" page instead of your app. If you check your running app's logs and see an error like `at=error code=H14 desc="No web processes running"`, then you have one more change to make to your Procfile.

If you look this error up (Heroku error 414- documented [here](https://devcenter.heroku.com/articles/error-codes#h14-no-web-dynos-running)), you'll likely read that this means your app is using 0 dynos (the Heroku abstraction for containers of running code). The suggested fix is usually to access your app via the Heroku CLI and scale up the number of web dynos from 0 to 1. However, we are likely encountering this error because of the custom Procfile we introduced to the project.

Normally, when you have no Procfile, Heroku determines the default actions it needs to take at each phase to get your app up and running. This means executing the framework-specific (Spring Boot in this case) equivalent of a "run" command. When we introduced our own Procfile, Heroku may no longer run those default commands, so if we don't specify a command to start the web server for the app ourselves then Heroku may never actually start the server for the app at all, which explains why there are "no web dynos running". We can remedy this by adding the following line to the Procfile:
```
web: java $JAVA_OPTS -Dserver.port=$PORT -jar target/*.jar
```

This just means to start up the app server and run the app's executable (compiled to `target`) on the specified port (probably defined as an environment variable somewhere within your app). This StackOverflow question was useful for solving this problem: <https://stackoverflow.com/questions/31484383/how-to-deploy-spring-boot-app-project-to-heroku>

Try deploying once more with this change to the Procfile. If you get the same `414` error, you should now login into the Heroku CLI on your terminal and run the following command (using your actual app name):
```
heroku ps:scale web=1 --app YOUR_HEROKU_APP_NAME
```

This is just the recommended general solution to the `414` error- it should start up a dyno and your app should hopefully start working after a refresh. Once you have a running app, click around to make sure everything works, particularly the parts you know are dependent on the changes introduced by the latest database migration. I recommend adding the line `spring.jpa.show-sql=true` to your `application.properties` file to keep an eye on all the SQL activity in your app's logs on Heroku.

If everything goes well, then congrats on running your (possibly) first Spring Boot migrations!

Other important notes:
* Each migration is run exactly once (unless specified otherwise- there are ways to specify that you want a migration to be repeatable). The status of each migration is tracked in Flyway's schema history table, which it adds automatically to your database. This means that when the migrate command is triggered, all migrations that have not previously run will be ordered and run, but you don't have to worry about the same migrations running each time you trigger the command.
* Don't try to name tables with reserved words
    * If you write a SQL migration that contains a reserved word as a column or table name, you will likely get a syntax error and the release will fail. You can get around this by always enclosing reserved words in quotes, which will allow you to *create* those tables or columns. However, the SQL commands generated by Spring Boot through reflection will not include those quotes (because it will assume the table names are all valid), so those queries will likely fail and crash your app. 
    * Here is a StackOverflow question about this issue: <https://stackoverflow.com/questions/22256124/cannot-create-a-database-table-named-user-in-postgresql>
* You might find that certain variable types that you used in your entity are not supported in SQL/ PSQL- simply look up the equivalent to use in your SQL command. For example, if you have a variable in an entity of type `long`, you should use type `bigint` in your corresponding migration for that table (if you write one). This change only needs to take place in your migration command- there is no need to change your entity.

Resources:

* <https://www.callicoder.com/spring-boot-flyway-database-migration-example/>
* <https://www.baeldung.com/database-migrations-with-flyway>
* Section 85.5 from: <https://docs.spring.io/spring-boot/docs/current/reference/html/howto-database-initialization.html>
* <https://devcenter.heroku.com/articles/running-database-migrations-for-java-apps>
