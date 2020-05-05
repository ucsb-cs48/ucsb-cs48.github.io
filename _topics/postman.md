---
topic: Postman
desc: "A tool for testing HTTP based APIs"
---

Many APIs are based on HTTP requests and responses.

Examples:
* The backend of a NextJS app
* The [GitHub API](https://developer.github.com/v3/)
* The [UCSB Developer APIs](https://developer.ucsb.edu/)

Postman is a useful tool for testing such APIs.  

It allows the user to format `GET`, `POST`, `PUT`, `DELETE` and other messages, including:
* setting HTTP headers
* putting content in the request body

It then allows you to inspect the response content, including headers and body.

#  How to set up postman to capture your Auth0 authentication cookie:

(Notes by Scott Chow)

* Install Postman here: <https://www.postman.com/downloads/>
* Open your locally running app (e.g. running on `localhost`)
* Inspect the page via `> Application > Storage > Cookies`
* Copy the `a0:session` cookie’s name and value into postman’s cookie setter
* Fire the request on postman to the app backend, see that it’s authenticated
