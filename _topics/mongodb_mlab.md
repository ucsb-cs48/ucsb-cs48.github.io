---
topic: "MongoDB: Mlab"
desc: "A cloud provider of MongoDB databases with a free tier"
category_prefix	: "MongoDB: "
indent: true
---

This page shows how to set up your account on MLab to use MongoDB.

For example Java code, see: <https://github.com/pconrad-webapps/simple-java-mlab-demo>

# Step 1: Create an mLab account on mlab.com

Go to <https://www.mlab.com> and follow the instructions on the website to create a mLab account. 
You will also a receive an email, asking you to confirm your account.

You'll create a username and password at this step, but note:
* the username/password you create here is NOT, I repeat NOT the one used to access the database!
* the username/password you create here IS the one you use to login to the mlab dashboard.

Make sure you understand this difference!   
* Your mlab username/password is used to control the mlab account.
* SEPARATELY, you create user/password combinations that are used to access individual databases

# Step 2: Creating a Database (i.e. a MongoDB Deployment)

Once you've created an account on mLab, we're create a deployment for your database. Make sure you are logged in and on your home page. 

1. Create a new database by clicking the `Create new` under the MongoDB Deployments.

2. You will then be taken to a different page to choose a provider and a plan. Make sure that the plan you choose is the one that is free, that is, the 'Sandbox' plan with up to 0.5GB of storage. For the provider, we will stick with the default of Amazon Web Services.

3. Next, you will be asked to choose a region. Select 'US East (Virginia)' and click the continue button.

4. You are then asked to choose a database name. Choose something that make sense for your application. For example, if your database is storing information about cars, call it `carsdb` or just `cars`. If you don’t have an application in mind, you can just use `testdb`. After choosing your name, click continue.

5. Finally, review all the details to make sure they're correct and click 'Submit Order'.

# Step 3: Using your MongoDB Deployment (database)

From the home screen of Mlab, you should see your database listed. Click on its name to bring up some more options, including adding a collection and adding a database user. 
 
You need to add a collection before you are ready to try using any code that accesses the database. 
But, you should have your repo ready to go with an `app.json`, `localhost.json`, or `heroku.json` file
ready to edit before you add the database user. Both of these statements are explained in more detail below.

## Adding a Collection

The properties that you need to set code for accessing an mLab database are these:

| property | explanation |
|----------|-------------|
|database name | One per application.  A MongoDB database can contain multiple collections, each of which contain multiple documents. |
|hostname | (e.g. `ds135912.mlab.com`) (Be sure to include the `.mlab.com` as well!)
| password | NOT your mlab password, but one you create per database |
| port number  | e.g. `35912` |
| username | NOT your mlab username, but one you create per database.  `dbuser` is fine.|

In Spring applications, these are set via the following properties, shown here in json syntax:

```json
{
 "spring.data.mongodb.database":"fill-in-database-name",
 "spring.data.mongodb.host":"fill-in-host",
 "spring.data.mongodb.password":"fill-in-password",
 "spring.data.mongodb.port":"fill-in-port",
 "spring.data.mongodb.username":"fill-in-username"
}
```

The hostname, port and database name can be found in the information at the top of the page 
for your MongoDB Deployment. Look for the text under 
“To connect using a driver via the standard MongoDB URI”. It should look a little something like this: 

```
mongodb://<dbuser>:<dbpassword>@YOURHOSTNAME.mlab.com:PORTNUMBER/DATABASE_NAME
```

Fill in the values you can.  For example, from this URL

```
mongodb://<dbuser>:<dbpassword>@ds143932.mlab.com:43932/cs56-m18-demo
```

you'd fill in these fields:

```
{
    "spring.data.mongodb.database":"cs56-m18-demo",
    "spring.data.mongodb.host":"ds143932.mlab.com",
    "spring.data.mongodb.password":"fill-in-password",
    "spring.data.mongodb.port":"43932",
    "spring.data.mongodb.username":"fill-in-username"
}
```

Note that dbuser and dbpassword are in `<>` and not given to you yet. 
The dbuser and dbpassword are things you’ll create in a step below.

# Creating the user and password

Think of a simple username such as `dbuser1`. Note, this is not a *human* user, but rather a "machine user",
i.e. it is the user/password credentials that will be used by your application code to connect to 
this database. 

Put this value into your properties file (e.g. `app.json`, `localhost.json` or `heroku.json`), for example:

```
 "spring.data.mongodb.username" : "dbuser1"
```

Next, your password will be random characters such as `weaf8jawel8f8waefjawe8fjlaw8fhalwifhaw3`. 
Type it in to the JSON first, then copy-and-paste that password (not literally the one on this web page) into the mLab 
user creation form.

```
 "spring.data.mongodb.password" : "weaf8jawel8f8waefjawe8fjlaw8fhalwifhaw3"
```

Copy the password value and go back to mLab. 
* Find the screen for your database where it has buttons labelled:

   | Collections | Users | Stats | Backups | Tools |
   
* Click the tab called 'Users' 
* Click the 'Add database user' button. 
* Use `dbuser1` (for example) for user, and paste the password in the corresponding fields. 

Now you should have a full set of credentials, something like this:

```
{
    "spring.data.mongodb.database":"cs56-m18-demo",
    "spring.data.mongodb.host":"ds143932.mlab.com",
    "spring.data.mongodb.password":"weaf8jawel8f8waefjawe8fjlaw8fhalwifhaw3",
    "spring.data.mongodb.port":"43932",
    "spring.data.mongodb.username":"dbuser1"
}
```

If later you want to access the database on another machine, and you don't 
have access to the original credentials, you can also make additional users,
`dbuser2`, etc.  If you are working on a team project with multiple team members,
you could make these usernames match the individual users on the project.

