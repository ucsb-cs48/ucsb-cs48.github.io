---
topic: "Testing: cypress"
desc: "End to end testing of Javascript web applications"
category_prefix: "Testing: "
indent: true
---


Cypress is a package that allows us to test web applications by automating
interactions with those applications in a real browser, or a simulated (headless) browser.

* Cypress Documentation: <https://www.cypress.io/>


# Example

As an example of Cypress testing, take a look at the files under `cypress/integration/*.spec.js` in the `project-idea-reviewer-nextjs` project:

* [cypress/integration](https://github.com/ucsb-cs48-s20/project-idea-reviewer-nextjs/tree/master/cypress/integration)

One of these files is called [`admin.spec.js`](https://github.com/ucsb-cs48-s20/project-idea-reviewer-nextjs/blob/master/cypress/integration/admin.spec.js)and its contents contain a series of tests where the code automates the
steps a user would take in interacting with the application.

Here is an explanation of the code a bit at a time:

```
describe("Admin Page", () => {
  before(() => {
    cy.prepareDatabase();
  });
  ...
```

The first line indicates that the entire `describe` block contains tests having to do with the `Admin Page`.

The `before` function contains the definition of a function that should be called once before all of the tests
in this block are executed.

The `cy.prepareDatabase()` function is defined in the file `cypress/support/commands.js` and contains code that
interacts with a special "backdoor" for resetting the database before running the test suite.  If your tests
involve database interactions where the database can be changed, or where the database needs to be in a certain state
for the tests to work properly, you may want need to incorporate this mechanism in your project as well.

```
   context("When I am logged in as an admin", () => {
       beforeEach(() => {
         cy.loginAsAdmin();
         cy.visit("http://localhost:3000/admin/admins");
       });
```

These lines indicate that all of the tests under the `context` block are in the context of being logged in as an admin.

Accordingly, there is a `beforeEach` that calls the function `cy.loginAsAdmin();` (defined in `cypress/support/commands.js`)
to login as an admin.  This login is "faked" using code that depends on `USE_TEST_AUTH` being defined; more on that later.

The `cy.visit("http://localhost:3000/admin/admins");` ensures that each test in this block starts on the page in the app
at the URL `admin/admins`.   Every other test is relative to the content of that page.

```
    it("shows me admin navbar options", () => {
      cy.get(".navbar-nav").contains("Ideas");
      cy.get(".navbar-nav").contains("Admin");
    });
    it("shows me a admin table", () => {
      cy.get("table");
    });
    it("shows me a admin form", () => {
      cy.get("form");
    });
```

The next few line contain tests.  Each of these tests starts with an `it` function call.  The first parameter
to `it` should be an sentence or phrase that describes what the web page being tested "should do".

The `cy.get` function calls try to find a certain element on the web page, either by HTML element name, or
CSS class, or by content.  The test passes if the element is found.  It fails if the element is not found.

The rest of the file contains many additional tests, including ones the illustrate filling in forms,
clicking on buttons, etc.

For any test that you may want to run, you can likely find example code that gives you an idea of how to run the test.

Getting Cypress set up initially is also straightforward.  In case you don't already have cypress in your project,
we'll cover how to get in incorporated below.

The hard parts, if you need them, are:

* authentication
* resetting the database

However, we have workable solutions to these problems, which we will also explain below.

# Getting Cypress into your Project

The following PR illustrates how to add cypress testing to a project:

* <https://github.com/ucsb-cs48-s20/cs48-s20-nextjs-tutorial/pull/3/files>

Here is a run down of the individual steps:

1. Add these lines to your `.gitignore`
   ```
   cypress/screenshots
   cypress/videos
   ```
   
   These directories are used by cypress to store screenshots and videos of your tests as they run.  You can use these
   to try to figure out "what went wrong" (or "what went right") in your tests.  But these are files created on the fly
   as you run cypress tests, not files that you want to commmit to your repo.
 
2. If your README.md has a table of commands to run the code, such as this one:

   ```
   | Command                | Description                                  |
   | ---------------------- | -------------------------------------------- |
   | `npm install`          | Install Dependencies                         |
   | `npm run dev`          | Runs locally                                 |
   ```

   then you'll want to add these two lines to the table so that the commands to run
   tests are documented.
   
   ```
   | `npm run test`         | Runs entire test suite                       |
   | `npm run test:cypress` | Runs Cypress integration tests               |
   ```
   
3. In the main directory, add a file `cypress.json` with the following contents:

   ```
   {}
   ```
   
   This is indeed just an *empty JSON object*.   There may be circumstances later where you may need to
   put stuff into this JSON object to configure cypress in various ways, but for now, the existence of this
   empty object is sufficient to get started.

4. Create the following files under the directories indicated:

   Under `cypress/plugins/index.js`:

   ```
   // See: https://on.cypress.io/plugins-guide
   module.exports = (on, config) => {};
   ```
   
   Under `cypress/support/commands.js`:

   ```
   // See: https://on.cypress.io/custom-commands
   ```
   
   Under `cypress/support/index.js`

   ```
   // See: https://on.cypress.io/configuration
   import "./commands";
   import "@rckeller/cypress-unfetch";
   ```

5. In `package.json` you'll need to add lines in a few places.

   Under `scripts`, add these lines if they don''t already exist.  Note that some of them may already 
   be there, in which case, don't duplicate them.

   ```
      "cy:run": "cypress run",
      "test": "npm-run-all test:*",
      "test:cypress": "start-server-and-test dev 3000 cy:run"
   ```

   Then, under `devdependencies`, add these lines.  Again, check for duplication; don't duplicate 
   any lines that already appear.
   
   ```
    "@rckeller/cypress-unfetch": "^1.0.1",
    "cypress": "^4.2.0",
    "npm-run-all": "^4.1.5",
    "prettier": "^2.0.5",
    "pretty-quick": "^2.0.1",
    "start-server-and-test": "^1.10.11"
   ```

6. Create a directory `cypress` in the main directory of the repo.  Then create `cypress/integration`, and under that
   add a file `home.spec.js` with your first cypress test.  Note that if the home page of your application
   does NOT have a `<nav class="navbar">` element on it, then you'll want to replace the test with one that makes
   more sense for your particular app.
   
   For example, if your page has an element `<h1>Dog Sitter App</h1>`, you could 
   * change `"has a nav bar"` to `"has an h1 containing Dog Sitter App"`
   * change  `cy.get("nav.navbar").should("exist");` to `cy.get("h1").should('have.text', 'Dog Sitter App');`
   
   ```
   describe("Home Page", () => {
     beforeEach(() => {
       // runs before each test in the block
       cy.visit("http://localhost:3000");
     });

     it("has a nav bar", () => {
       // a nav element with class navbar
       cy.get("nav.navbar").should("exist");
     });

   });
   ```
   
7. Run `npm install` to install your new `devdependencies`.
8. Try running `npm run test:cypress` for the first time.

You should see that your first cypress tests runs, and with luck, also passes.

# Do you need to do anything else?

If all of the functions that you want to test meet the following criteria, then the steps above may
be sufficient for your needs:

* The tests you are running do not require the user to login (e.g. via Auth0).
* The tests you are running do not require the user to have a specific "role" in your app (e.g.
  being a regular user vs. being an admin).
* The tests do not involve updating the database in any particular way; either there is no database,
  or the database only contains fixed data that is not updated or changed during the test.
 
If that's where you are, you are ready to continue writing Cypress tests just based on the setup
you already have.  The documentation at: <https://docs.cypress.io/guides/overview/why-cypress.html#In-a-nutshell> should
be sufficient to guide you through the process.
 
On the other hand:

* If you *do* need the user to be logged in or have a specific role, then you'll need to
  consult the additional information below on how to set up mock authentication for your app.
* If your tests will be updating the database, you'll need the database to start in a known
  predictable state at the start of each of your test runs.  For that, consult the information 
  in the section below.


# Authentication / Authorization / Roles, and Resetting the Database

In order to write cypress tests that depend on the user being logged in with a particular role, especially when the authentication is delegated to a service such as Auth0 or another OAuth provider, we would either have to
* (a) hard code username / passwords of real users into our cypress tests (not advisable)
* (b) find a way to "get the appllication into the same state as if" a real user had logged in and had authenticated.

As it turns out that also depends on having the database in a certain stated.

The code in this section depends on a particular application architecture, one that is illustrated 
in the repo:

* <https://github.com/ucsb-cs48-s20/project-idea-reviewer-nextjs>

The following pull request details steps that we took to put in place a scheme for
* mocking authenticated users
* resetting the database to a known state via an endpoint.

<https://github.com/ucsb-cs48-s20/project-idea-reviewer-nextjs/pull/45/files>

This is a complex pull request involving changes to 28 files, so it isn't particularly easy to summarize, but we'll try to do our best.

## Examples of Tests we want to support

Perhaps it's best to start with what we were trying to accomplish in the first place.   Here is the start of a cypress test suite that tests admin functions, that is functions that are only available to an `admin` user of the app.

An admin user is defined as one that has an entry in MongoDB database, in the `users` collection, with `"role" : "admin"`.  The user is identified by their email address.  For example:

```json
{
  "email": "cgaucho@ucsb.edu",
  "role": "admin"
}
```

The test suite `cypress/integration/admin.spec.js` starts like this:

```
describe("Admin Page", () => {
  before(() => {
    cy.prepareDatabase();
  });
  context("When I am logged in as an admin", () => {
    beforeEach(() => {
      cy.loginAsAdmin();
      cy.visit("http://localhost:3000/admin/admins");
    });
    it("shows me admin navbar options", () => {
      cy.get(".navbar-nav").contains("Ideas");
      cy.get(".navbar-nav").contains("Admin");
    });
    it("shows me a admin table", () => {
      cy.get("table");
    });
```

Taking this a bit at a time, we see that in the code below, there is a `before` action that calls `cy.prepareDatabase();`.  This is a custom cypress command (i.e. one that we've written ourselves) that sends messages to the application to say: please reset the database to a known state for testing.    As we'll see later, this command will fail unless the environment variable `USE_TEST_AUTH` is set.    The `USE_TEST_AUTH` environment variable is used to indicate that we want to use "test authentication" instead of real authentication, and that we want to use a test instance of the MongoDB database instead of the real one.

```
describe("Admin Page", () => {
  before(() => {
    cy.prepareDatabase();
  });
```  

The next bit of code shown below sets a context for a set of test that are run when logged in as an admin.  There is a `beforeEach` action that runs another custom cypress command (one that we've written ourselves) that puts the application in a state "as if" the current user has logged in as an admin.   This runs before each test in this section:

```
  context("When I am logged in as an admin", () => {
    beforeEach(() => {
      cy.loginAsAdmin();
      cy.visit("http://localhost:3000/admin/admins");
    });
``` 

This is followed by a series of tests that depend on the context of being logged in as an admin.  They are expected to pass in that context.  For example:

```
    it("shows me admin navbar options", () => {
      cy.get(".navbar-nav").contains("Ideas");
      cy.get(".navbar-nav").contains("Admin");
    });
    it("shows me a admin table", () => {
      cy.get("table");
    });
    ...
```

Later on, there are two other context blocks that show the other roles: the role of a student user (a user that has an email that appears in the student table), and a guest user (a user with an email that is in neither the admin table, nor the student table.)

Each of these also starts with a custom cypress command that sets up the type of user:

For students, it tests that trying to visit the admin page simply redirects the user to the home page:

```
   context("When I am logged in as a student", () => {
    beforeEach(() => {
      cy.loginAsStudent();
      cy.visit("http://localhost:3000/admin/admins");
    });

    it("cannot visit the admin page", () => {
      cy.url().should("eq", "http://localhost:3000/");
    });
  });
```

For guests, the test is similar, so it is not shown.

## The custom commands we need

The custom commands we need in order to support these tests are these:

* `cy.prepareDatabase()`, which is called in a `before` action at the top of an entire test suite file (e.g. at the top of each of these files to reset the test database to a known, predictable state:
   - `cypress/integration/auth.spec.js`
   - `cypress/integration/admin.spec.js`
   - `cypress/integration/home.spec.js`
   - `cypress/integration/student.spec.js`
   - and any future `*.spec.js` files added under `cypress/integration`
* `cy.loginAsAdmin();`, `cy.loginAsStudent();` and `cy.loginAsGuest();`, each of which is called in a `beforeEach()` at the top of a `context` block for tests run as an admin, student, or guest user, respectively.

The custom commands are defined in the file: `cypress/support/commands.js`, which is shown here:

```
import adminUser from "../fixtures/adminUser.json";
import studentUser from "../fixtures/studentUser.json";
import guestUser from "../fixtures/guestUser.json";

Cypress.Commands.add("loginAsAdmin", () =>
  cy.setCookie("AUTH", JSON.stringify(adminUser))
);
Cypress.Commands.add("loginAsStudent", () =>
  cy.setCookie("AUTH", JSON.stringify(studentUser))
);
Cypress.Commands.add("loginAsGuest", () =>
  cy.setCookie("AUTH", JSON.stringify(guestUser))
);

Cypress.Commands.add("prepareDatabase", () => {
  cy.visit("http://localhost:3000/testhooks");
  cy.get("button").contains("Prepare Database").click();
  cy.get("span").contains("Database has been reset; ready to run tests.");
});
```

The first three commands are implemented by setting a cookie called `AUTH` to have the contents of *fixtures*, which are data structures used for testing.  The fixtures are in the directory `cypress/fixtures/`, and contain the JSON for three different cases of users.  The data placed in the cookie simulates the same data that *would have been placed there by Auth0* if real authentication were being used.

<table class="{table table-sm table-striped table-bordered">
<thead>
<tr>
<th><code>../fixtures/adminUser.json</code></th>
<th><code>../fixtures/studentUser.json</code></th>
<th><code>../fixtures/guestUser.json</code></th>
</tr>
</thead>
<tbody>
<tr>
<td>
<pre>
{
  "name": "Example Admin",
  "email": "admin@example.com"
}
</pre>
</td>
<td>
<pre>
{
  "name": "Example Student",
  "email": "student@example.com"
}
</pre>
</td>
<td>
<pre>
{
  "name": "Example Guest",
  "email": "guest@example.com"
}
</pre>
</td> 
</tr>
</tbody>
</table>

The final commmand, `prepareDatabase` is set up to interact with an endpoint called `http://localhost:3000/testhooks`.  At this endpoint, we expect to see a button labelled `Prepare Database`.  When we click on this button, we expect to see the text `Database has been reset; ready to run tests.`.

As we'll see later, this page and this button are only available in the app when the `USE_TEST_AUTH` 

The button `Prepare Database`, when pressed, sends a `POST` message to an API endpoint called `/api/testhooks/prepareDatabase`, which, when `USE_TEST_AUTH` is enabled, will run code that cleans out all of the collections in the database, and then inserts into the `users` collection only the minimal database records needed so that the `cy.loginAsAdmin()` and `cy.loginAsStudent()` commands will work.  (Note that by definition, nothing needs to be in the database to support the `cy.loginAsGuest()` command.)   That api endpoint returns an error if `USE_TEST_AUTH` is not enabled; as a result, if someone tries to post to the endpoint in the production app, the `POST` is simply rejected.

It should be noted that it was, strictly speaking, *not necessary* nor even good practice to make a UI element for this endpoint.   As the cypress documentation notes, it is [not a best practice to use the UI to set up state for a test](https://docs.cypress.io/guides/getting-started/testing-your-app.html#Bypassing-your-UI).   Instead, we could have implemented the `cy.prepareDatabase()` command by simply using `cy.request()` to directly interact with the `/api/testhooks/prepareDatabase` endpoint, doing the appropriate `POST` request, and checking the status code.   (Refactoring that to be in line with best practices is left as an exercise for later students and/or course staff.)

## Additional Changes

TODO: Document the additional changes needed to get this working, including but not limited to:

* changes involving `USE_TEST_AUTH` in both the code and the `package.json`
* changes involing `MONGODB_URI_TEST`
* changes to `utils/api.js` and `utils/ssr.js`, and addition of `utils/testAuth.js`
