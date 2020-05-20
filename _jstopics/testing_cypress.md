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
   
4. Create a directory `cypress` in the main directory of the repo.  Then create `cypress/integration`, and under that
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
   
5.    
   
# Getting Authentication / Authorization / Roles to work

TODO: WRITE THIS

# Resetting the database

TODO: WRITE THIS



