---
topic: Next.js
desc: "A framework based on node and React"
category_prefix: "nextjs: "
---

next.js is a framework for building webapps based on:
* the JavaScript language
* the node server side framework
* the express.js web server
* the React front-end framework

The main website for next.js is here: <https://nextjs.org/>

For clarification:

* Nextjs is a framework that uses:
  - React (a library of javascript functions/classes that is useful for UI) and 
  - Node (a runtime for javascript that allows you to run javascript outside of a browser). 
  
The language being written in is javascript, in both cases.

When people say they’re writing “in React”, they really mean they’re writing in javascript using React.


# Troubleshooting strategies

Sometimes, you may need to "regenerate" the files that are automatically created by the next.js ecosystem.

There isn't a universal `npm clean` built into the `npm` ecosystem (similar to a `make clean`).

But, if you need to do something like a `make clean` or a `mvn clean`, the equivalent would be something like this:

```
rm -rf .next node_modules
```

After doing this, you'll need to do the following to regenerate these files:

```
npm install
npm run dev
```
