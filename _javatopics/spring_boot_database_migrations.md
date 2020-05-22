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
  ...
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

Then add the wrapper downloader files to your repo (they must be part of the source code that is deployed to Heroku so the wrapper can be installed and the commands can be properly run):

```
git add mvnw .mvn
```
