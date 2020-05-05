---
topic: "MongoDB: NextJS Setup"
desc: "Configurig your NextJS app for MongoDB"
category_prefix	: "MongoDB: "
indent: true
---

# Connecting to MongoDB

To connect to MongoDB, you'll need to do the following.  (Note: if you want to have separate databases for development, qa and production, there are additional instructions after these simple ones).
* Define `MONGODB_URI` in your `.env` file
* In your `next.config.js` file, where you see:
  ```
  AUTH0_DOMAIN: process.env.AUTH0_DOMAIN,
  ```
  Add a line into that same structure like this one:
  ```
   MONGODB_URI: process.env.MONGODB_URI,
  ```
* Add a line like this into `utils/config.js`, but *only* in the part for settings exposed to the *server*.
  LEAVE THIS OUT of the part for settings exposed to the *client*, otherwise you'll have a security leak.
  ```
  MONGODB_URI: process.env.MONGODB_URI,
  ```
* In `utils/mongodb.js`, the file has a hardcoded value of `database`, which assumes the name of your database
  is `database` (literally).  If it already exists under a different name, then change this line, changing `database`
  to the correct name.
  ```
    return client.db("database");
  ```
  If you don't have a file `utils/mongodb.js`, you can get it from [`project-idea-reviewer-nextjs`](https://github.com/ucsb-cs48-s20/project-idea-reviewer-nextjs/blob/master/utils/mongodb.js)

## Multiple Databases (for dev/prod/qa)

To use different databases for dev/prod/qa)

1. In `.env` create three different variables as follows, and put in the mongodb URIs for the three databases:
   ```
   MONGODB_URI=mongodb+srv://adminuser (put in full URI of dev database here)
   MONGODB_URI_STAGING=mongodb+srv://adminuser... (put in full URI of qa database here)
   MONGODB_URI_PRODUCTION=mongodb+srv://adminuser... (put in full URI of production database here)
   ```
2. Change the code in `next.config.js` to match this code:

   Before the `module exports = {` line, paste in this function definition.  This chooses a value for
   the MongoDB URI from among the three provided, based on the value of another environment value, namely
   the value of `NODE_ENV`:
   
   ```javascript
   function mongodb_uri() {
     if (process.env.NODE_ENV === "production") {
       return process.env.MONGODB_URI_PRODUCTION;
     } else if (process.env.NODE_ENV === "staging") {
       return process.env.MONGODB_URI_STAGING;
     }
     return process.env.MONGODB_URI;
   }
   ```
   
   Then change: 
   ```
   MONGODB_URI: process.env.MONGODB_URI,
   ```
   
   to:
   ```
   MONGODB_URI: mongodb_uri(),
   ```

3. Use `npx heroku-dotenv push --app your-heroku-app` for both prod and dev

4. Manually set the following variable on the Heroku settings config vars:
   * Set `NODE_ENV` to `production` for the production site
   * Set `NODE_ENV` to `staging` for the QA site

