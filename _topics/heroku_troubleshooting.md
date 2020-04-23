---
topic: "Heroku: Troubleshooting"
desc: "Solutions to common problems and errors"
category_prefix: "Heroku:"
indent: true
---

# Seeing the logs for your app

To see a continuous flow of the logs for your app:

```
heroku logs --tail --app app-name-goes-here
```

(requires `heroku` command line app, which is installed on CSIL.  Use `heroku login -i` to login through a CSIL ssh session.)

# When using `mvn heroku:deploy`

Note: this is a workflow based on including a heroku `plugin` in the `pom.xml` file.

**We have not been using this workflow recently** in CS56 and CS48, but some tutorials
might include it, so I'm leaving this in the documentation.

Error: `Could not find app name: No 'heroku' remote found.` 

Symptom: You use `mvn heroku:deploy` and see:

```
[ERROR] Failed to execute goal com.heroku.sdk:heroku-maven-plugin:2.0.3:deploy 
(default-cli) on project spring-boot-github-oauth-demo01: 
Failed to deploy application: Could not find app name: No 'heroku' remote found. -> [Help 1]
```

Possible Causes, and Solutions:

* Make sure you are logged into the heroku cli
   * Solution: `heroku login`
* Make sure that the correct heroku app is defined in your `pom.xml`
   * Solution: list your apps with `heroku apps` then check the value of `<appName>` in the `heroku-maven-plugin`
* No remote is defined for heroku
   * To check, type `git remote -v` and see if a remote for `heroku` is listed.  If not:
   * To add it, use `heroku git:remote --app app-name` (replace `app-name` with the name of your app)
         
