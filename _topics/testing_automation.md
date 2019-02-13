---
topic: "Testing: Automation"
desc: "How to make testing an automatic part of your process"
category_prefix: "Testing: "
indent: true
---

The best way to ensure that testing becomes a part of your process is to automate it.

One way of automating this is to ensure that each time you commit code to your project, an entire suite of tests is run automatically.

There is a buzzword for that approach: it's called **continuous integration**.   

The process of ensuring that your code base stays sane is sometimes called "keeping the build green" as described in this article:
* [Why you should keep your build green](https://www.petrikainulainen.net/programming/unit-testing/why-you-should-keep-your-build-green/), an article on Continous Integration by [Petri Kainulainen](https://www.petrikainulainen.net/about-me/).

# Travis-CI

An easy way to get started with continuous integration is with Travis-CI. 

* Travis-CI is a platform that offers free continuous integration services for any open source project on Github.
* It is straightforward to set up automated testing on Travis-CI for dozens of programming languages including all those commonly used in UCSB CS courses:

| Language/Platform | Link to Travis-CI documentation |
|-------------------|---------------------------------|
| Android | <https://docs.travis-ci.com/user/languages/android/> |
| C | <https://docs.travis-ci.com/user/languages/c/> |
| C++ | <https://docs.travis-ci.com/user/languages/cpp/> |
| Java | <https://docs.travis-ci.com/user/languages/java/> |
| JavaScript with node.js | <https://docs.travis-ci.com/user/languages/javascript-with-nodejs/> |
| Python | <https://docs.travis-ci.com/user/languages/python/> |
| Ruby | <https://docs.travis-ci.com/user/languages/ruby/> |
{:table }

There are many more; this is only a partial list.  Visit <https://docs.travis-ci.com/user/languages> for the full list.
