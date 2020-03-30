---
topic: "OAuth: Google Setup"
desc: "Setting up a Google OAuth App to obtain client id and client secret"
category_prefix: "OAuth: "
indent: true
---


# How to set up Google OAuth for `localhost`

1. Navigate to <https://developers.google.com/identity/sign-in/web/sign-in> to create a Google OAuth Application.
    - If you are asked "Where are you calling from", select "Web Server"

    - Set the *Authorized Redirect URI* to: `http://localhost:8080/login/oauth2/code/google`
2. Add the following items to your `secrets-localhost.properties` file, filling out the `client-id` and `client-secret` with the values from  your Google OAuth application
   ```
   spring.security.oauth2.client.registration.google.client-id: PUT-CLIENT-ID-HERE
   spring.security.oauth2.client.registration.google.client-secret: PUT-CLIENT-SECRET-HERE
   ```
3. Run `mvn spring-boot:run`

# How to set up Google OAuth for Heroku

1. Navigate to <https://developers.google.com/identity/sign-in/web/sign-in> to create a Google OAuth Application.
    - If you are asked "Where are you calling from", select "Web Server"
    - Set the *Authorized Redirect URI* to: `https://your-app-name.herokuapp.com/login/oauth2/code/google`
2. Add the following items to your `secrets-heroku.properties` file, 
   filling out the `client-id` and `client-secret` with the values from  your Google OAuth application
   ```
   spring.security.oauth2.client.registration.google.client-id: PUT-CLIENT-ID-HERE
   spring.security.oauth2.client.registration.google.client-secret: PUT-CLIENT-SECRET-HERE
   ```
3. Run the script `setHerokuEnv.sh --app your-app-name`

   If you don't have a `setHerokuEnv.sh` script for your app, create one that looks like this:

   ```
   #!/usr/bin/env bash
   heroku config:set HEROKU_PROPERTIES="$(cat secrets-heroku.properties)" "$@"
   ```
4. Deploy the app on Heroku
   
  
