---
topic: "Testing"
desc: "Everything having to do with testing: Unit testing, Integration Testing, Test Coverage"
category_prefix: "Testing: "
---

The article [The Testing Introduction I Wish I Had](https://dev.to/maxwell_dev/the-testing-introduction-i-wish-i-had-2dn) by [Max Antonucci](https://dev.to/maxwell_dev) is a great introduction to testing (and according to [dev.to](https://dev.to) is a 14 minute read).

It divides the topic of testing into six areas.  The first three of these are  core areas of testing that most treatments of the topic mention:

| Test type | Explanation |
|-----------|--------------|
| Unit Tests | "the simplest test[s] for the smallest possible pieces of your program." |
| Integration Tests | "check how well separate units [of the code] integrate together" |
| Acceptance Tests | "shift away from what pieces of code should do to what users should do. These tests are based around common user tasks like logging in, submitting a form, navigating content, and having their privacy invaded by tracking scripts. This usually makes acceptance tests the highest-level tests for any application, and often the most important." |

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

