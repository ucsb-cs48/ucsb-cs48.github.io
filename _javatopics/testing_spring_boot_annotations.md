---
topic: "Testing: Spring Boot Annotations"
desc: "When to use certain annotations and what they do"
category_prefix: "Testing: "
indent: true
---

## `@RunWith(SpringRunner.class)`
Required to use Spring Boot features such as `@MockBean` in testing classes. Should generally always be included.

## `@SpringBootTest`
Tells Spring to look for the main `@SpringBootApplication` and run the tests in that context. This annotation also lets you configure specific features with additional parameters. This runs the entire Spring Boot application and runs the test against that instance.

For example, `@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT, properties = foo.bar=true)` will run the Spring Boot instance on a random port with the environment variable `foo.bar` set to true.


Note: Configure with `properties="spring.datasource.name=XYZ"` where `XYZ` is some unique string to start the test case with a fresh database loaded from `data.sql`

## `@Before` (JUnit4) / `@BeforeEach` (JUnit5)
Indicates a setup function that will run before every individual test function. This is useful for setting up variables that will need to be reset before each test case

## `@BeforeClass` (JUnit4) / `@BeforeAll` (JUnit5)
Indicates a setup function that will run once before the entire test class. This is useful for setting up variables that will only need to be initialized once before it is used by all of the test cases.