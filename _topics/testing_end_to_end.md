---
topic: "Testing: End to End Testing"
desc: "Intro to End to End Testing, and Framework Specific Examples"
category_prefix: "Testing: "
indent: true
---

# What is end to end (or acceptance) testing? How does it differ from other tests?

Generally, there are three kinds of tests in a web application:

- Unit tests: tests a specific component of an application in isolation. Everything the component depends on is [mocked](/topics/testing_mocking/) (hence testing in isolation). An application typically has more unit tests than integration and end to end tests.
- Integration tests: tests how two or more components interact with eachother. Some dependencies (such as databases) may or may not be mocked here.
- End to end tests: tests how the application works as a "black box" without mocking anything. An application generally has the fewest amount of end-to-end tests compared to integration or unit tests.

The idea behind end-to-end tests is that we want to mimic how a user would interact with an application as a whole to make sure all the pieces work together correctly. We don't want to mock anything (except external dependencies such as OAuth2, more on this later). In the context of a web app, this involves using a headless browser to click through the application and assert that the correct information is shown on the screen. Selenium is the testing framework we use for Spring Boot projects, and Cypress is used for NextJS/React.

For example, a single end-to-end test might take the following steps in an automated browser (cypress/selenium):
- Navigate to homepage of app
- Verify login button exists
- Click the login button
- Make sure user is redirected to login service
- Enter credentials and click log in 
- Make sure user is redirected to the app and the app recognizes the user
- Perform several actions while using assert() to make sure the app correctly renders the expected result each time

## End to end testing with 3rd party services
Generally, tests should almost never be accessing 3rd party services. Using 3rd party services in tests increases test complexity (requiring API keys to be configured on CI, possible rate limiting, network issues, tests failing due to a service being unreachable) and thus it is typically avoided. Instead, the URLs of 3rd party services can have their return values mocked during testing so they always return an expected result without reaching out to the actual internet. The process of doing this will vary by framework, but [Wiremock](http://wiremock.org/) can be used for Spring Boot apps. The general process of mocking a 3rd party service includes setting up a mock endpoint and using an environment variable so the application knows whether to use the real or mock endpoint to fetch data during runtime.

## End to end testing apps that require OAuth2 authentication
Testing applications that require OAuth2 authentication is typically a significant roadblock when testing applications. At the surface, it seems like such a difficult problem to solve since it relies so closely on a 3rd party service (such as Google OAuth2) where you need real user credentials to proceed. However, mocking authentication is very similar to mocking any other 3rd party service. The process for achieving this varies by framework but we have specific examples in Spring Boot and NextJS that make it easy to incorperate into to another project.

# Framework-specific Examples

## End to end testing with Spring Boot and Selenium

[This file from project-idea-reviewer](https://github.com/ucsb-cs48-s20/project-idea-reviewer/blob/master/src/test/java/edu/ucsb/cs48/s20/demo/end2end/UserFlowEnd2EndTest.java) is a great starting point for creating an end-to-end test with authentication. 

Instead of relying on Google OAuth, we set specific environment variables for this test (`spring.security.oauth2.client.provider.wiremock.*` and `spring.security.oauth2.client.registration.wiremock`) to configure Wiremock as a mock OAuth2 provider and client. Wiremock provides a fake sign in page and returns a valid user when visited, so we can test our app with a valid user.

In the [setup](https://github.com/ucsb-cs48-s20/project-idea-reviewer/blob/master/src/test/java/edu/ucsb/cs48/s20/demo/end2end/UserFlowEnd2EndTest.java#L68) function, we configure our fake OAuth2 provider's response to the authentication request. We can specify the returned user's information (such as email address). Their role will be fetched from the database which is seeded from the [data.sql](https://github.com/ucsb-cs48-s20/project-idea-reviewer/blob/master/src/main/resources/data.sql) file. If you wanted to run the tests as an admin user, you could add a new user to the `data.sql` file and return that user's email from the `setup` function from within the test. The setup function also contains code to initialize the WebDriver (which is basically an automated chrome browser) we can control with our test cases.

Now within test functions such as [runUserFlowEnd2EndTestWithAuthentication()](https://github.com/ucsb-cs48-s20/project-idea-reviewer/blob/master/src/test/java/edu/ucsb/cs48/s20/demo/end2end/UserFlowEnd2EndTest.java#L102), you can use methods like:
* `webDriver.get(url);` to direct the browser to a specific page 
* `webDriver.findElement(By.id("submit")).click();` to click a button
* `assert(webDriver.findElement(By.id("headerText")).getText().equals("Welcome!"));` to assert a certain text field contains the correct value

NOTE: It is highly recommended to reset the database between each test suite. This can be done by adding `properties="spring.datasource.name=xyz"` to the `@SpringBootTest` annotation (where `xyz` is a unique string). An example of this can be seen on [line 28](https://github.com/ucsb-cs48-s20/project-idea-reviewer/blob/master/src/test/java/edu/ucsb/cs48/s20/demo/end2end/UserFlowEnd2EndTest.java#L28). The database will be reloaded based on `data.sql`.

## End to end testing with NextJS and Cypress

See: <https://ucsb-cs48.github.io/jstopics/testing_cypress/>
