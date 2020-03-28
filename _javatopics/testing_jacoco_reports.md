---
topic: "Testing: Jacoco Reports"
desc: "How to interpret the reports (red, yellow, green)"
indent: true
category_prefix: "Testing:"
---

# How to read the test coverage reports

* If any line of code is red, that means it is not tested at all&mdash;it is being missed by *line coverage*
* If a line of code is yellow, it means there are multiple ways to execute the line.
   * it may have an if/else, or a boolean expression involving `&&` or `||`, and thus there are multiple paths through the code (multiple branches).  
   * Yellow means it is being missed by *branch coverage*; some branches are covered, and others are not.   
   * Think about the multiple paths through the code and be sure your tests are coverage all of them.

For more on line vs. branch coverage, see: <https://ucsb-cs56-pconrad.github.io/topics/testing/>
