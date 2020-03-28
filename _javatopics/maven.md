---
topic: Maven
desc: A build tool for Java plus a package manager
category_prefix: "Maven: "
---

The short version:

* Maven adds package management to the functions that you get with Ant.
* By package management, I mean that instead of you having to manage the process of obtaining all the .jar files for third party library that
  your project may depend on (and the .jars that *those* files depend on, and so on...), maven manages that for you.
* Maven is built on a different philosophy than Ant, explained below.

# Similarities with Ant

* Both Ant and Maven provide a way to manage the process of compiling, making jars, producing javadoc, running your code.
* Both use XML as the format
* Built in Java, so its ostensibly portable like Java (Makefiles are very Unix centric)
* Both are maintained by the Apache Software Foundation

# Superficial differences between Maven and Ant:

* `pom.xml` instead of `build.xml`
* command to run the tool is `mvn` instead of `ant`
* The directory structures for Ant is somewhat flexible, but Maven has definite expectations.  
    * With Ant, you can decide how to organize your directories, and configure your build.xml file accordingly
    * With Maven, you need to do things Maven's way, or you'll be sorry.    We'll ok, so strictly speaking
      [that's not true, according to the Maven FAQ](https://maven.apache.org/general.html#dir-struct), but it certainly
      is easier to just organize things according to the Maven defaults.
* The pom.xml syntax is a bit more complicated than the build.xml syntax (okay, that's a personal opinion)
    * But, you get more power, so it's perhaps worth it?
* Maven will manage a lot more things for you than Ant does, including downloading and caching the .jar files your 
  application needs.
     * You still have to specify what .jars those are in `dependency` declarations in the `pom.xml` file.

# The big difference: imperative vs. declarative.

Ant's `build.xml` is *imperative*. 

* The creator of the `build.xml` file, has to understand both *what* you want to do (e.g. compile some Java code), and *how* to do it (e.g. with the `javac` task.
* The `build.xml` author creates targets that specify both what you want to do, and how.

Maven's `pom.xml` is *declarative*.   

* The `pom.xml` is a way to configure *dependencies*, and specify which *plugins* are being used, and how they are configured.
* The *plugins* provide *goals*, which are the things that the programmer wants to accomplish (like compiling, making a jar, publishing javadoc, etc.)   
* In a `pom.xml`, you say only what you want to do, and specify a bit of configuration information, but Maven decides *how* to accomplish each goal, using it's plugins. 
* The plug-ins all use a set of common conventions and expectations, so that, in principle, there isn't that much configuration you have to do for most common tasks.

The biggest advantage of the declarative style is when you have dependencies.  If you know you depend on something, say `junit` or `apache-commons` or `batik-svggen`, you just grab the bit of code that "declares" the dependency, and it will automatically go get the .jar file for you, and put it in the right place in your classpath for compiling, creating javadoc, testing, making a jar, etc.

The biggest drawback is that if/when you are trying to do something *very specific* like put a file in a specific directory (e.g. putting your javadoc under `docs`, because that's where github-pages expects them to be), you may find yourself fighting with Maven a bit.  Maven wants to put things where *it* wants to put them, and expects you as the developer to just submit to its will.


# References:

* [Apache Maven home page](https://maven.apache.org/)
* [Apache Maven FAQ](https://maven.apache.org/general.html)
* [Apache Maven in 5 minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)    
 

# Remove custom files (e.g. `logs`) in `mvn clean`

* If you want `*.log` from the `logs` directory (or any other custom fileset or directory) to be deleted when you do `mvn clean`, here is how you can accomplish that: just "configure the `maven-clean-plugin`" in your pom.xml (as explained [here](https://maven.apache.org/plugins/maven-clean-plugin/examples/delete_additional_files.html)).  Be sure this goes in the `build` plugsin (not the `reporting` plugins, for example.)

```xml
   <!-- also remove logs on maven clean -->
      <plugin>
        <artifactId>maven-clean-plugin</artifactId>
        <version>3.1.0</version>
        <configuration>
          <filesets>
            <fileset>
              <directory>logs</directory>
              <includes>
                <include>**/*.log</include>
              </includes>
            </fileset>
          </filesets>
        </configuration>
      </plugin>
```
