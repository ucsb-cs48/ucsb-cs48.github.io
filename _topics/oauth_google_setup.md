---
topic: "OAuth: Google Setup"
desc: "Setting up a Google OAuth App to obtain client id and client secret"
category_prefix: "OAuth: "
indent: true
---

# Setting up Google OAuth for a web app

Before you start, you need the name of your app, e.g. `cs48-cgaucho-lab00`.

You'll get back two values: a *client-id and a *client-secret*.  You'll store those somewhere in the configuration for your app (exactly where depends on the tech stack you are using).

# Setting up a Google OAuth app from the "button"

If you visit the page <https
://developers.google.com/identity/sign-in/web/sign-in> there is a button that looks like this:

![Configure a Project Button](00-configure-a-project-button.png)

In the context of the page, it's here: 

![Button in Context](00-button-in-context-50pct.png)

Click this button. You'll be walked through the following sequence of steps:

![01-select-create-a-new-project.png](01-select-create-a-new-project.png)

![02-enter-new-project-name.png](02-enter-new-project-name.png)

Here, you are being asked for the name of for the app you are setting up
- This name merely allows you to tell one app from another in the Google control panel
- This might be, for example, `cs48-cgaucho-lab00` or `cs48-s20-ride-share`
- It's often the same as the name of your app on your hosting service (e.g. Heroku or now.sh)

![03-specify-the-product-name.png](03-specify-the-product-name.png)

Here, you are being asked for the name that will be displayed to the user during the login flow

This doesn't have to be the same as the previous name, but it's easiest if you make it the same.

![04-enter-redirect-url.png](04-enter-redirect-url.png)

Here, you are being asked for one or more authorized redirect uris.  The exact uri(s) will depend on your platform

* For a Spring Boot app on localhost, it will be <tt>http://localhost:8080/login/oauth2/code/google</tt>  
  - Note that this is `http` (not `https`)
* For a Spring Boot app called `cs48-cgaucho-lab00` running on Heroku, it will be <tt>https://<i>cs48-cgaucho-lab00</i>.herokuapp.com/login/oauth2/code/google</tt>
  - Note that this should be `https` (not `http`)
* You may be able to list both the `localhost` and the heroku uri, separated by commas.

![05-get-client-id-and-client-secret.png](05-get-client-id-and-client-secret.png)

At this step, you are provided the *client-id* and *client-secret*

You can use the buttons to copy these to the clipboard and then paste them where they belong.
* The details of where they belong depends on your specific tech stack.
* For Spring Boot apps, these typically go into 
  - `secrets-localhost.properties` file, like this:
    ```
    spring.security.oauth2.client.registration.google.client-id: PUT-CLIENT-ID-HERE
    spring.security.oauth2.client.registration.google.client-secret: PUT-CLIENT-SECRET-HERE
    ```
  - `secrets-heroku.properties` file, in the same way.
* Some repos also have a script called `convertGoogleCredentials.py`; in this case, you can download
  the secrets to a file called `configuration.json`, and run the script to produce the secrets in a format
  suitable for copying and pasting into the applicable `.properties` file.

* An additional step is necessary for Spring Boot on Heroku; to copy the values from your 
  `secrets-heroku.properties` file to Heroku, you need to run the script `setHerokuEnv.sh`, with
   a parameter `--app your-app-name` where `your-app-name` is the app name on Heroku.
   
   For example:
   
   ```
   ./setHerokuEnv.sh --app cs48-s20-cgaucho-lab00
   ```

   If you don't have a `setHerokuEnv.sh` script for your app, create one that looks like this:

   ```
   #!/usr/bin/env bash
   heroku config:set HEROKU_PROPERTIES="$(cat secrets-heroku.properties)" "$@"
   ```
   
## Using the script `convertGoogleCredentials.py`  

When setting up Google OAuth, you can download the credentials into a file called `credentials.json`.

The Python script `convertGoogleCredentials.py` available in the demo repo: <https://github.com/ucsb-cs48-s20/demo-spring-google-oauth-app> can be used to convert the `credentials.json` file to the format used in Spring Boot properties files.


# `console.developers.google.com`

You can manage existing Google OAuth projects from the url <https://console.developers.google.com/> 
