---
topic: "MongoDB: NextJS Guide"
desc: "How database operations in NextJS differ from examples in standard node"
category_prefix	: "MongoDB: "
indent: true
---

In generic tutorials on accessing MongoDB from node, you may see examples such as this one (taken from the [W3Schools Node MongoDB documentation for a query with `find`](https://www.w3schools.com/nodejs/nodejs_mongodb_query.asp)

```
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var query = { address: "Park Lane 38" };
  dbo.collection("customers").find(query).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```

Here's what you should know about this example: *most of it doesn't apply to NextJS* but you can still use some of it as a reference.

The parts that *do not apply*  are these:

```
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  ...
  dbo.collection("customers")
```

All of that is code to set up the connection to the database, which *done differently* in NextJS apps.

For example, here is the code to get access to a particular collection, done NextJS style:

```
import { initDatabase } from "../../../utils/mongodb";

export async function getStudents(section) {
  const client = await initDatabase();
  const users = client.collection("users");
  ...
```

Now, on the other hand, the code that happens *after* we connect with a particular collection, that code may be useful as
a reference.  So be aware as you use example from online how to translate what you see there into something relevant.



