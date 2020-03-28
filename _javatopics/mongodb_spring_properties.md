---
topic: "MongoDB: Spring Properties"
desc: "How to set properties for connecting to MongoDB when using Spring"
category_prefix	: "MongoDB: "
indent: true
---

When using MongoDB with Spring, you can set the connection properties using the property settings below
(these were copied from [this list of common Spring application properties](https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html) on 11/27/2018)

# Typical properties

Note that the `spring.data.mongodb.uri=` property doesn't allow 
can't be set along with the host, port, or credentials (username or password).

```
spring.data.mongodb.database= # Database name.
spring.data.mongodb.host= # Mongo server host. Cannot be set with URI.
spring.data.mongodb.password= # Login password of the mongo server. Cannot be set with URI.
spring.data.mongodb.port= # Mongo server port. Cannot be set with URI.
spring.data.mongodb.username= # Login user of the mongo server. Cannot be set with URI.
```

# Complete list of properties

```
# MONGODB (MongoProperties)
spring.data.mongodb.authentication-database= # Authentication database name.
spring.data.mongodb.database= # Database name.
spring.data.mongodb.field-naming-strategy= # Fully qualified name of the FieldNamingStrategy to use.
spring.data.mongodb.grid-fs-database= # GridFS database name.
spring.data.mongodb.host= # Mongo server host. Cannot be set with URI.
spring.data.mongodb.password= # Login password of the mongo server. Cannot be set with URI.
spring.data.mongodb.port= # Mongo server port. Cannot be set with URI.
spring.data.mongodb.repositories.type=auto # Type of Mongo repositories to enable.
spring.data.mongodb.uri=mongodb://localhost/test # Mongo database URI. Cannot be set with host, port and credentials.
spring.data.mongodb.username= # Login user of the mongo server. Cannot be set with URI.
```

