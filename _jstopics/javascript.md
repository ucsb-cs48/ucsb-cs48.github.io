---
topic: "JavaScript"
desc: "Getting Started with Learning JavaScript"
layout: default
category_prefix: "JavaScript: "
---


JavaScript is important for web development for at least two reasons:

1. Nearly all client-side web programming in done in JavaScript, because there is pretty much no alternative. 
2. Some server-side programming is also done in JavaScript, using the `node.js` framework, for example.

There are at least three separate aspects of "learning JavaScript" that you may want to consider separately, and/or in relationship to one another.

1. Learning JavaScript *as a programming language*, in the abstract.
2. Learning JavaScript *as it is used, in practice* for client-side web application programming.
3. Learning JavaScript *as it is used in `node.js` for server-side web programming, or for command line applications.

Here are a few resources for learning JavaScript, and where each one is useful.

# Resources for Learning JavaScript

* Head First JavaScript, [on campus](http://proquest.safaribooksonline.com/book/programming/javascript/9781449340124), [off-campus](http://proquest.safaribooksonline.com.proxy.library.ucsb.edu:2048/book/programming/javascript/9781449340124)

* Many, many other JavaScript books in the O'Reilly Digital Library [on campus](http://proquest.safaribooksonline.com/search?q=javascript), [off-campus](http://proquest.safaribooksonline.com.com.proxy.library.ucsb.edu:2048/search?q=javascript)

# How do I even get started?

## In a web browser

If you want to work with JavaScript in a web browser, the best way is to start with a simple web page,
and put JavaScript into a `<script>` element, like this:

```html
<!DOCTYPE html>
<html>
<head>
  <script>
  /* Your code goes here */
  </script>
</head>

<body>
  <p>This is a web page</p>
</body>
</html>
```
You can create that file with an `.html` extension in any editor, and then open it with any browser.   You don't need any kind of server, or special account, or anything.   You can do it on any computer where you have an editor (e.g. WordPad, TextEdit, etc. though something like vim, emacs or SublimeText is probably better since it will do syntax highlighting and auto-indentation.)

The "compiler/interpreter" is actually built-in to the web browser.   There is also a debugger built into the "JavaScript Console" of some web browsers, which you can often bring up with CTRL/SHIFT/J (Window/Linux) or CMD/SHIFT/J (on Mac).

## Using `node.js`

If you work on a CSIL machine, and/or if you install "node" on your own computer, then you can use the `node` command to enter a JavaScript command line interpreter (Read/Eval/Print Loop).

Here's a sample of that from CSIL.  Note that `console.log` is sort of like a "print" function, but since it's return value is undefined, we see the value `undefined` after each invocation of `console.log` (after its does its job of printing something.)

```
-bash-4.3$ node
> console.log("Hello World")
Hello World
undefined
> console.log(2 + 2)
4
undefined
> "foobar".toUpperCase()
'FOOBAR'
> 
```

# JavaScript related topics

* [JQuery](/topics/jquery)
    * <http://www.cs.ucsb.edu/~pconrad/cs130e/examples/jquery/gettingStarted/>
