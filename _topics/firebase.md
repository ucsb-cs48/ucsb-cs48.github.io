---
topic: "Firebase"
desc: "A Google sponsored app development platform"
category_prefix	: "Firebase: "
---

Firebase is an interesting platform that provides various services for application development.
It is primarily for mobile app development.

# Using Firebase with Heroku

If you have a firebase.json file that has all of the authorization info in it, you should NOT commit that to github.

Instead, use a script such as this one: [setHerokuEnv.py](https://github.com/ucsb-cs56-pconrad/spring-boot-github-oauth-demo03/blob/master/setHerokuEnv.py) which takes a JSON file and stores all of the key value pairs into a Heroku app.

From the app on Heroku, you can use whatever means your programming langauge has for accessing environment variables to get the values. For example, in Python, you use code [like this](https://devcenter.heroku.com/articles/getting-started-with-python#define-config-vars)

# Tutorials

* <https://www.tutorialspoint.com/firebase/>

A "Cloud Firestore" Java tutorial (i.e. the Firebase database) oriented towards CS56 students (though it used SparkJava instead of Spring Boot).  Written by Fuheng (Charlie), a mentor from CS56 M18.

* <https://github.com/ucsb-cs56-webapps/ucsb-cs56-qrcodereader/blob/master/Cloud_Firestore_tutorial.md>


# Getting Started

* Go to <https://firebase.com> and login with your Google Credentials.
* Create an app, and accept the terms.
* That will put you at the Firebase Console
* You'll see buttons for iOS, Android, and one that looks like this: `</>`.  
   That last one means "webapp".
   
# Firebase with Java

These links has information about using Firebase with Java:

* <https://firebase.google.com/docs/admin/setup>
* <https://github.com/firebase/firebase-admin-java>

