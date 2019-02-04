---
topic: JQuery
desc: "A library that makes JavaScript easier to use."
tags:
- JavaScript
---

JQuery is a library of useful JavaScript objects and methods that makes it easier to work with the elements that make up a web page.

That's a plain english way of saying that JQuery provides a set of objects and methods for working with the Document Object Model (DOM).  The DOM is nothing more than a standard set of objects (with methods and attributes) that describe all of the HTML elements in a web page.    

There are many ways of working with the DOM from a variety of programming languages.  JQuery is one of the mostly commonly used ways of accessing the DOM from JavaScript code.

In JQuery, almost everything you do is accessed through a single function that has the name `$`.  That's right, "dollar sign" is the name of the function.  You call it like this:

```javascript
   $();
```

However, that's a stupid way of calling it, because you aren't passing in anything as a parameter, and you aren't doing anything with the returned value.  Far more common is something like this:

```javascript
$(document).ready(function(){
  $("#theButton").click( function(){
    var contents = $("#theTextArea").val();
    var howManyLines = countNewLines(contents);
    $("#theResultsGoHere").html(
      "<p>There are " + howManyLines + " lines</p>"
    ); // end of html function 
  }); // this ends the call to the .click function
}); // this ends the call to the .ready function
```

For an explanation of that example, see: <http://www.cs.ucsb.edu/~pconrad/cs130e/examples/jquery/gettingStarted/>

Use "view source" in a web browser to look at the source of the web page. You'll find the JavaScript code inside one of the `<script>` tags.


# References

* [Main JQuery Website](https://jquery.com/): <https://jquery.com/>

# Simple examples

* <http://www.cs.ucsb.edu/~pconrad/cs130e/examples/jquery/gettingStarted/>



