---
topic: "Testing"
desc: "Everything having to do with testing: Unit testing, Integration Testing, Test Coverage"
category_prefix: "Testing: "
---

The article [The Testing Introduction I Wish I Had](https://dev.to/maxwell_dev/the-testing-introduction-i-wish-i-had-2dn) is a great introduction to testing   It is written by [Max Antonucci](https://dev.to/maxwell_dev) and according to [dev.to](https://dev.to) is a 14 minute read.

A good second article for those working on front-end development is this one: [Static vs Unit vs Integration vs E2E Testing for Frontend Apps](https://blog.kentcdodds.com/static-vs-unit-vs-integration-vs-e2e-testing-for-frontend-apps-ec4eb7e855d1).

This article takes the perspective that it is better to learn the testing pyramid from the top, instead of the bottom: <https://github.com/NoriSte/ui-testing-best-practices/blob/master/sections/beginners/top-to-bottom-approach.md>

# Three core test types

Antonucci's article divides the topic of testing into six areas.  The first three of these are  core areas of testing that most treatments of the topic mention:

| Test type | Explanation |
|-----------|--------------|
| Unit Tests | "the simplest test[s] for the smallest possible pieces of your program." |
| Integration Tests | "check how well separate units [of the code] integrate together" |
| Acceptance Tests | "shift away from what pieces of code should do to what users should do. These tests are based around common user tasks like logging in, submitting a form, navigating content, and having their privacy invaded by tracking scripts. This usually makes acceptance tests the highest-level tests for any application, and often the most important." |
{:.table .table-sm .table-striped .table-bordered}

Here's a bit more on each of these:

## Unit tests

Writing good unit tests has some different conventions from writing production code.  
* This article discusses some of those: <https://mtlynch.io/good-developers-bad-tests/>

The gist is that unit tests should be simple and modular. Each test should focus on some very specific aspect of the function or object you are testing and should not be dependent on any other parts of the application working. 
* For example, if you were testing a webapp, you might want to test that errors are added to the model when passing an invalid value to a controller action. This controller action might require access to a database or to an authentication service in order to work during runtime, but you want to avoid any reliance on anything that doesn't directly pertain to what you are trying to test. 

* That is, you don't want to check "Are there errors on my model after I authenticate the current user and query the database for this value?" for this unit test. You want to check "Assuming the user is already authenticated and this value does not exist in this database, will the correct errors properly be added to the model?" 
  * This first scenario is not a unit test- you would essentially be testing that authentication, a particular database query, *and* that the errors are properly added.
  * The second scenario is a unit test because it is *only* checking that the errors are properly added in a scenario that would cause them to be added. You would achieve this modularity by mocking any services that the test would otherwise rely on- the database and authentication, in this example. (Article on mocking coming soon!)

There is also the idea of self-documenting unit tests. This is where best test-writing practices differ from best development practices. Each test should have a name that thoroughly identifies its purpose, even if it is a really long name. 
 * For example, a name like `testLogin_nonStudentNotAllowedToLogin` is often preferable to a more concise name like `nonStudentLogin` because it makes the following three things very clear:
   * Which function is being tested (`login`)
   * The case that is being checked (a non-student attempting to login)
   * The expected outcome (they should not be allowed to do so). 
   
This way, if the test fails for any reason, it will be easy for anyone to identify the exact details of what is going wrong just from seeing the error message. 

Test code is also a lot more repetitive than production code- you generally don't have helper functions, since you want a reader to see everything that is being set up and used in the body of the test itself. Sometimes you can pull out some setup that is required for every single test to a single set up function that is run before every test, but this should be written in a way that is very clear to any reader. In general, choose readability over efficiency in test code, even if that means more repetition or copy-pasting.


Unit tests in JavaScript:
* <https://areknawo.com/lets-talk-js-unit-testing/>

## Integration Tests

Integration tests examine how multiple components work with each other. Once you have a thorough set of unit tests, you can be confident that each individual part of your project works well on its own. However, you rarely just have those components working alone, so you need to check that they behave correctly when interacting with one another.

This article discusses the main approaches to integration testing: <http://softwaretestingfundamentals.com/integration-testing/>

In general for integration testing, you are gradually combining components together to verify that they work properly together, so it is easier to catch errors in specific interactions.

 * For example, if you had three components (A,B,C), you would first look for "interfaces" between those components- places where two components interact. Test each of these interfaces before testing interactions that involve all of the components. 
   * In this example, say A first passes some information to B, which creates an object with the data and passes the object to C, which saves it to a database. The interfaces here are between (A,B) and (B,C).
    * When A passes some information to B, make sure that information is received correctly in B, and that B returns the correct type of object with the expected values. If necessary, mock C so that the only "real" interaction that occurs is between A and B. If this test fails, then you know that something went wrong with the information passed from A to B.
    * Repeat this process with B and C, to make sure that they communicate properly amongst themselves (and that C saves the correct information to the database).
    * Once each "piece" of the A-B-C interaction is checked, then test interactions that include all 3 components, as they would interact in an actual running app

For integration tests, you may need more than a simple test framework.

For front-end testing of Web Apps, for example, or any kind of app built with React:
* <https://testdriven.io/blog/modern-frontend-testing-with-cypress/>

## Acceptance Tests (aka end-to-end tests)

The last of these is also sometimes called "end-to-end" testing when it is automated.  In Antonucci's article, acceptance testing refers to automated tests of acceptance criteria: for example, for web apps, these tests might be automated using a "headless brower".

These are related to the acceptance tests that might be carried out by a human that is looking at acceptance criteria on a user story or issue.     In both cases, these might be specified using the "GIVEN/WHEN/THEN" style of writing acceptance tests.   Ideally, these tests should be *both*:
* tested by a human when they are reviewing a pull-request that is claimed to resolve/fix/impelement an issue/user-story/bug-fix.
* automated and placed into a suite of tests run each time code is commmited to the project (the idea of "continuous integration").

# Three other test types

[Max Antonucci](https://dev.to/maxwell_dev)'s article [The Testing Introduction I Wish I Had](https://dev.to/maxwell_dev/the-testing-introduction-i-wish-i-had-2dn) also mentions three additional important areas of testing:

| Test type | Explanation |
|-----------|--------------|
| Visual Regression Testing | "for unexpected (or expected) visual changes in the app".  Compares before and after screenshots of the app as it runs, pixel-by-pixel |
| Accessibility Testing | Tests for accessibility of apps for users with different abilities (e.g. low-vision, color-vision-issues, blind users that interact with screen reading software). |
| Code Quality Tests | Using linters to look through a code base for issues such as code duplication, security risks, style conventions (e.g. indenting), overly complex control structures, etc. |
{:.table .table-sm .table-striped .table-bordered}

# Code Quality Tests

The last of these three, "code quality tests" deserves a special mention, especially pretty-printers and linters.

* *pretty-printer or *pretty-printing* is a technical term for programs that do automatic code formatting.  
* *linter* or *linting* is a technical term for programs that do *static analysis* on program source code to find code quality problems.    
By *static analysis*, we mean that the program parses and analyzes the code, similar to the way a compiler does, but rather than generating executable code or bytecodes, or directly carrying out the program, it analyzes the code *without executing it*.  That is what makes this *static* analysis as opposed to *dynamic* analysis of program behavior (*dynamic* implies that we execute the code in order to measure its behavior).

There are a variety of programs to do this kind of analysis for the languages typically used in CS48.

For Python:

* Linters:
   * [`flake`](https://hackingthelibrary.org/posts/2018-02-08-lint/) 
   * [`pylint`](https://www.pylint.org/) also discussed here: [How Python Linters Will Save Your Large Python Project](https://jeffknupp.com/blog/2016/12/09/how-python-linters-will-save-your-large-python-project/) 
   * [`black`](https://pypi.org/project/black/) is a code formatter 
   * [`mypy`](http://mypy-lang.org/) is a static type checker for Python
   * The github repo [`facebookincubator/ptr`](https://github.com/facebookincubator/ptr/blob/master/README.md) discusses a Python Test Runner that incorporates running a test suite and computing test coverage along with running both `black` and `mypy`

For JavaScript:
   * [`eslint`](https://eslint.org/docs/user-guide/getting-started)

# Other Test Types

The term *smoke test* or *sanity test* is sometimes used to refer to a test that is run during staging (i.e. putting a new version of an application into production).  It is a "fast test that is done by a script or human that ensures that the application under test works to minimal expectation. For example, a human run smoke test involves logging into the app and doing some usual activities such as conduct a search or exercise a standard feature."

The idea is that if the team has done a reasonable job of testing the code base, then it is sufficient to do a quick test to make sure that all of the pieces are up and talking to each other (the various processes, servers, databases, APIs, etc.)  If a major component is failing, then it will show up quickly during a properly designed smoke test.

* Source: <https://www.mabl.com/blog/software-testing-in-staging-phase-of-deployment>



# Test Driven Development (TDD)

See also: [/topics/TDD/]({{ '/topics/tdd/' | relative_url }})

* <http://www.agiledata.org/essays/tdd.html>
* <https://www.agilealliance.org/glossary/unit-test>


# Test Coverage

It's helpful to be able to measure how much of our code is covered by tests.  This metric is known as "test coverage"

# Line Versus Branch Coverage

Two common metrics are

* line coverage (how many lines of our code are covered by tests)
* branch coverage (how many branches, i.e. directions we can go, are covered by tests.)

It might be immediately obvious why those are not the same.

The answer is that not every if statement has an else.

```
  color="blue";
  if (x<10) {
      color="red";
  }
  foo(color);
```

Suppose we have a test that covers the case where `x<10` evaluates to true.   Then for those code, we have 100% line coverage, but we 
do not necessarily have 100% branch coverage unless we ALSO have a test that covers the case when `x<10` evaluates to false.   That means
that there is a branch into calling `foo(color)` when `color` still has the value "blue", and that branch is untested.

# Tools for measuring test coverage

## Python

In Python, there is a module called `coverage` that can be installed with `pip`.


## JavaScript

In JavaScript, when using the node `npm` ecosystem, there is a module called `istanbul` that can be use used to measure code coverage.

## Java

In Java, `Jacoco` (http://www.jacoco.org/jacoco/index.html) is one tool for measuring test coverage.

The [documentation for Jacoco](http://www.jacoco.org/jacoco/trunk/doc/index.html) can be difficult to follow.

Here is some help:

* https://www.codeproject.com/Articles/832744/Getting-Started-with-Code-Coverage-by-Jacoco
* http://www.baeldung.com/jacoco
* https://github.com/powermock/powermock/wiki/Code-coverage-with-JaCoCo

