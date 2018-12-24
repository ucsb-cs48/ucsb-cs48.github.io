---
topic: "OAuth"
desc: "The way we implement the 'login with Google, Facebook, or Github' thing you see on some websites"
---

When implementing a login feature (i.e. usernames/passwords, and a "logged in" vs "not logged in" status), we have two choices:
* Roll our own authentication, storing the usernames/passwords ourselves.
* Delegate that to some other identify provider.

The second choice is almost always preferable.    Maintaining a database of usernames/passwords is an invitation to be hacked.  And, even
if your webapp is nothing special, people have a bad habit of reusing passwords across many different websites.

A common way of delegating authentication is via a protocol called OAuth.   
This is the protocol that allows the "login with Facebook", "login with Google", etc. functionality
you see on many websites.

# OAuth in the context of CS56

In CS56, we'll typically be dealing with OAuth in the context of using it in combination with [SparkJava web applications](/topics/spark_java/).

That involves using a package called [Pac4j](/topics/pac4j/).  Pac4J is a library for authentication in Java that tries to provide a common interface to many different authentication mechanisms (not only OAuth).    There are then extensions to Pac4J for particular platforms, including SparkJava.

Please see these links for more information on:
* [Pac4J](/topics/pac4j/)
* [SparkJava:Authentication](/topics/spark_java_authentication)
