---
topic: "Testing: jest"
desc: "Simple unit testing of plain old JavaScript functions"
category_prefix: "Testing: "
indent: true
---


Jest is package for unit testing in JavaScript.

* Jest Documentation: <https://jestjs.io/docs/en/getting-started>


# Example

As an example of plain old unit testing, consider the function `reformatEmail` defined in [`utils/email.js`](https://github.com/ucsb-cs48-s20/project-idea-reviewer-nextjs/blob/b3b0aeecd5af97f6bc4e8307e0b245589de27ed5/utils/email.js#L1) in the `project-idea-reviewer-nextjs`:

```javascript
function throwError(message) {
  throw new Error(message);
}

export function reformatEmail(email) {
  typeof email === "string" || throwError("email should be of type string");
  email || throwError("email should not be an empty string");
  return email.replace("@umail.ucsb.edu", "@ucsb.edu");
}
```

The function `reformatEmail` is pure functional javascript code that could be used anywhere in front end or back end code:
* The function has no side effects
* The function calculates a value based solely on it's parameters

When you have the opportunity to factor some logic out into such a function, it is straightforward to write test cases using
a tool called `jest` to unit test this code.  Here is an example of test cases for this code, from the file [`__tests__/email.js`](https://github.com/ucsb-cs48-s20/project-idea-reviewer-nextjs/blob/57ffad1b78e5cdde5b64c81723cec2b9785ec45a/__tests__/email.js#L1)


```javascript
import { reformatEmail } from "../utils/email";

describe("utils/email", () => {
  describe("reformatEmail", () => {
    it("converts @umail.ucsb.edu to @ucsb.edu", () => {
      expect(reformatEmail("tkomarlu@umail.ucsb.edu")).toBe(
        "tkomarlu@ucsb.edu"
      );
    });

    it("throws an error when parameter is not of type string", () => {
      expect(() => {
        reformatEmail(42);
      }).toThrow("email should be of type string");
    });

    it("throws an error when parameter is an empty string", () => {
      expect(() => {
        reformatEmail("");
      }).toThrow("email should not be an empty string");
    });
  });
});
```

A few notes:

* The `describe` blocks here are used to structure a test suite.  They have no effect other than
  - scoping for things such as `before` and `beforeEach` function that can be used to set up test cases
  - messages that appear in the test output to group related tests together.
* The `it` blocks give a name to each test.  The intention is that the parameter is a sentence or phrase starting with `it`
  that describes what the feature being tested is supposed to do.
* The other functions such as `expect`, `.toBe` and `.throw` are documented here: <https://jestjs.io/docs/en/getting-started>

When jest is configured in a project, you can run jest tests like this:

```
project-idea-reviewer-nextjs % npm run test:jest 

> project-idea-reviewer-nextjs@0.1.0 test:jest /Users/pconrad/github/ucsb-cs48-s20/project-idea-reviewer-nextjs
> jest

 PASS  __tests__/email.js
  utils/email
    reformatEmail
      ✓ converts @umail.ucsb.edu to @ucsb.edu (2ms)
      ✓ throws an error when parameter is not of type string (1ms)
      ✓ throws an error when parameter is an empty string (1ms)

Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
Snapshots:   0 total
Time:        1.142s, estimated 3s
Ran all test suites.
project-idea-reviewer-nextjs % 
```

# Configurating a project for jest

To configure your project for jest testing, you will need to take these steps:

1. In the root of your project, create a file called `jest.config.js` with these contents:

   ```
   module.exports = {
     transform: {
       "^.+\\.jsx?$": "babel-jest",
     },
     testPathIgnorePatterns: ["/node_modules/", "/cypress/"],
   };
   ```

   This tells jest to
   * ignore files under `/node_modules` (since it's not our responsibility to run those tests; those
     are the modules imported when we do `npm install`)
   * ignore files under `/cypress` (those tests get run when we run cypress testing`
   
2. In `package.json`, add this line under `scripts`:

   ```
      "test:jest": "jest",
   ```

   This enables the `npm run test:jest` command to work.

3. If you do not already have it, add this:

   ```
       "test": "npm-run-all test:*",
   ```

   This makes `npm test` run all of the commands that start with `test:` including `test:jest`, but potentially
   also `test:cypress` and `test:format` if those are defined (and any other tests you may have.)

4. In the `devdependencies` section of your `package.json`, add these lines:

   ```
    "jest": "^25.4.0",
    "npm-run-all": "^4.1.5",
   ```

   Then run `npm install`

5. If you do not already have it, add a file called `.babelrc` with the following contents:

   ```
   {
     "presets": ["next/babel"]
   }
   ```

   This ensures that babel knows how to properly interact with jest, cypress, storybook, etc.
   
6. You should now be able to run `npm test:jest` and see your tests cases under `__tests__` run and produce output.

   If that's not the case, ask the staff for help.

