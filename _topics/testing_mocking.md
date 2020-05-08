---
topic: "Testing: Mocking"
desc: "Intro to Mocking in Tests, and Framework-specific Examples"
category_prefix: "Testing: "
indent: true
---

# What is mocking, and why do we need it in tests?

This article is a good introduction to the use of mocks: <https://circleci.com/blog/how-to-test-software-part-i-mocking-stubbing-and-contract-testing/>

The general idea is that, when testing (especially unit testing), you want to isolate your components so that each test examines exactly one thing. 

Say you have a function that calls three other services before returning its value. In a unit test, you might only want to check that the function returns the proper value, and you wouldn't want to have to first set up those three services and assume they work perfectly in order to see if this function works properly. Instead, you would *mock* those services. Mocking gives you more control over the exact conditions when testing components that are dependent on other components.

* A mock is a fake version of an object or a service that you can explicitly tell how to behave in a test.

For example, if you wanted to test the previously described function, you would create a mock object for each of those three services and tell them to return some value so that they all appear to be working properly for the function you are testing. This way, you can precisely test the behavior of this function under specific conditions. You can tweak the behavior of the mocks for different test cases- for example, to check that the function behaves in a certain way when two of the services run properly and one fails (where you would just program the mock of that service to return some failing value). 


# Framework-specific Examples

## Mocking with JavaScript testing using Jest

Coming soon

## Mocking in Java/ Spring Boot

The most common mocking framework used for Java unit tests is Mockito.
* This article is a quick introduction to using Mockito with Junit: <https://www.springboottutorial.com/spring-boot-unit-testing-and-mocking-with-mockito-and-junit>

You can use Mockito to create mock objects directly, which you can then pass to your services in your unit tests. You then set the mock's "responses" to specific actions using the `when()` function. 
For example, if you had created the mock object `foo` and wanted it to return `true` whenever the member function `bar` is called with the arguments `args`, you would write ` when(foo.bar(args).thenReturn(retval);`. You'd want to do this in a test where you know that `foo.bar` is going to be called with those arguments, and you simply want to return a hardcoded value rather than actually enter the `foo` class and run the function.

In Spring Boot, you can also use the `@MockBean` annotation to indicate that all occurrences of that bean will be replaced with the mock. You can use the `when()` function the same way as a regular mock, but you are no longer dealing with a specific mock object each time.
* This is the documentation for the `@MockBean` annotation: <https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/mock/mockito/MockBean.html>
* This post discusses some of the differences between `@Mock` and `@MockBean`: <https://stackoverflow.com/questions/44200720/difference-between-mock-mockbean-and-mockito-mock>
* You can see an example of `@MockBean` being used in this test file: <https://github.com/ucsb-cs48-s20/project-idea-reviewer/blob/master/src/test/java/edu/ucsb/cs48/s20/demo/controllers/AdminControllerTest.java>
