---
topic: "Maven: FAQ"
desc: "Frequently Asked Questions"
indent: true
---

# What the heck: `[WARNING] Using platform encoding (UTF-8 actually)`

Here's what to do:

```xml
 <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
```

While you are at it, you might also include the version of Java you need:

```xml
   <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
```
