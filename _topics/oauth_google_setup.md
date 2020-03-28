---
topic: "OAuth: Google Setup"
desc: "Setting up a Google OAuth App to obtain client id and client secret"
category_prefix: "OAuth: "
indent: true
---


# How to set up Google OAuth

1. Navigate to <https://developers.google.com/identity/sign-in/web/sign-in> to create a Google OAuth Application.
    - If you are asked "Where are you calling from", select "Web Server"

    - Set the *Authorized Redirect URI* to: `http://localhost:8080/login/oauth2/code/google`
2. Add the following items to your `localhost.json` file, filling out the `client-id` and `client-secret` with the values from 
your Google OAuth application:
```
"spring.security.oauth2.client.registration.google.client-id":"ENTER-GOOGLE-ID",
"spring.security.oauth2.client.registration.google.client-secret":"ENTER-GOOGLE-SECRET"
```
    - **NOTE**: If you want Heroku to also support Google OAuth, you'll need to do the same in `heroku.json`, hopefully with a set of production credentials from a production Google OAuth app.
3. Run `source env.sh` to update your environment.
4. Run `mvn -P localhost spring-boot:run` and see that your application is now configured to support Google OAuth!.
