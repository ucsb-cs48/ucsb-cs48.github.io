---
topic: "Code Style: checkstyle"
desc: "code style checker"
---

`checkstyle` (documentation here: <https://checkstyle.sourceforge.io/>) is a automated style checker that can be used on Java code.

When deployed on a Java project, it can be used to audit a code base for consistent style (e.g. indentation rules)
when making commits and pull requests.   CI/CD systems such as GitHub Actions can be used to automate running the checks.

# Incorporating Checkstyle into a Java Maven Project

To try out checkstyle, first add this `<plugin>` to the `<build>` section of your `pom.xml`. Note that:
* To run `checkstyle`, use `mvn checkstyle:check`
* If you run this on a legacy code base, you might get a ton of violations the first time you run it, and it may be
  a bit overwhelming.
* Don't panic.  We'll discuss how to cut this list down to something managable so that you can address them
  a little bit at a time.

```xml
		<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-checkstyle-plugin</artifactId>
				<version>3.1.1</version>
				<configuration>
					<configLocation>sun_checks.xml</configLocation>
					<encoding>UTF-8</encoding>
					<consoleOutput>true</consoleOutput>
					<failsOnError>true</failsOnError>
					<linkXRef>false</linkXRef>
				</configuration>
				<executions>
					<execution>
						<id>validate</id>
						<phase>validate</phase>
						<goals>
							<goal>check</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
```

For example, the first time we ran this on the `project-idea-reviewer` repo, we got 777 checkstyle errors:

```
...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  5.158 s
[INFO] Finished at: 2020-05-19T09:30:00-07:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-checkstyle-plugin:3.1.1:check (default-cli) on project demo-spring-google-oauth-app: Failed during checkstyle execution: There are 777 errors reported by Checkstyle 8.29 with sun_checks.xml ruleset. -> [Help 1]
...
```

Instead of tackling these all at once, perhaps a better strategy is to cut down the number of rules to a single rule, and
then try to add back in rules one at a time.  To do that, we'll need to know how to deploy a custom style file instead of
using one of the default ones built in to `checkstyle`.

# A Strategy for deploying `checkstyle`

The value of `<configLocation>` (which is under `<configuration>`) indicates the location of an XML file that 
defines which rules are enforced by `checkstyle`.  For example, this value indicates that the file `sun_checks.xml` is to
be used.   

```xml
<plugin>
  ...
  <configuration>
    <configLocation>sun_checks.xml</configLocation>
    ...
  <configuration>
  ...
<plugin>
```	  
	
The file `sun_checks.xml` is one that's built into the `checkstyle` product, and is documented here: <https://checkstyle.sourceforge.io/sun_style.html>.  
	
Another option is `google_style.xml` documented here: <https://checkstyle.sourceforge.io/google_checks.html>.
You can swap out `sun_checks.xml` for `google_checks.xml` and get a different report.  
	
One major difference
between the two is that while the `sun_checks.xml` file results in blocking errors, the `google_checks.xml` file
results only in warnings.
	
Both `sun_checks.xml` and `google_checks.xml` can be specified without adding an additional file beyond adding
the plugin to the `pom.xml`.  However, you can also download a copy of either of these files and then tailor it 
to your own needs and preferences.
	
The originals of these files can be found in the GitHub repo for `checkstyle` here:
* <https://github.com/checkstyle/checkstyle/tree/master/src/main/resources>
	
Copy one of these files into the main directory of your repo (or, if you prefer, under a `checkstyle` direcctory, e.g.
`checkstyle/checks.xml`) and specify that file, e.g.     
```	
	<configLocation>checkstyle/checks.xml</configLocation>
```	

Start with the total contents of one of the files, and then you can start editing down (or commenting out) the rules
to a minimal, manageable, set.  For example, here is a minimal version that started with `google_checks.xml` and made
these changes:

* Changed `<property name="severity" value="warning"/>` to ` <property name="severity" value="error"/>`.  
	
  This change makes it so that a violation, when incorporated into CI/CD systems such as GitHub Actions, will
  result in a red X (failed check) rather than a green check (passed check).
	
* Reduced the list of `<module>` definitions inside the outer `<module>` to one, namely the `FileTabCharacter` 
  module documented at <https://checkstyle.org/config_whitespace.html#FileTabCharacter>.   This module simply
  checks to see whether there are any tab characters in the source code.  

	
```xml
<?xml version="1.0"?>
<!DOCTYPE module PUBLIC
          "-//Checkstyle//DTD Checkstyle Configuration 1.3//EN"
          "https://checkstyle.org/dtds/configuration_1_3.dtd">

<module name = "Checker">
    <property name="charset" value="UTF-8"/>
    <property name="severity" value="error"/>
    <property name="fileExtensions" value="java, properties, xml"/>

    <!-- Checks for whitespace                               -->
    <!-- See http://checkstyle.org/config_whitespace.html -->
    <module name="FileTabCharacter">
        <property name="eachLine" value="true"/>
    </module>

</module>
```

With this in place, we can run and see the number of violations:

```
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.061 s
[INFO] Finished at: 2020-05-19T11:02:50-07:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-checkstyle-plugin:3.1.1:check (default-cli) on project demo-spring-google-oauth-app: Failed during checkstyle execution: There are 227 errors reported by Checkstyle 8.29 with checkstyle/checks.xml ruleset. -> [Help 1]
```

# Fixing Style Problems with Astyle

Fortunately, fixing these is straightforward using another tool called `astyle`, which is available on CSIL, and which can also be installed for Mac, Windows and Linux.  (See the CS48 article on [astyle](https://ucsb-cs48.github.io/javatopics/code_style_astyle/) for more info on installation options.) 

In fact, astyle can be used to automatically fix quite a few style issues required by the Google code style for
java(<https://google.github.io/styleguide/javaguide.html>).   If you have astyle installed, you can simply run
this command to fix all `.java` files under `src`.   

```
astyle --style=google --recursive src/\*.java
```

Notes: 
* This will change files in place, saving a backup copy of the original 
  as `filename.orig` (e.g. `Foo.java` becomes `Foo.java.orig`). 
* Consider adding `*.orig` to your `.gitignore` so that you don't end up committing 
  all of the backup files
  produced by `astyle` into your repo.
* It's advisable to be on a clean version of master, and to do this in a single commit 
  and pull request;
  you don't want to have to code review "substantive" changes in the same PR as a massive reformatting.
 

Note that the line `<property name="fileExtensions" value="java, properties, xml"/>` indicates that all `.xml` files and `.properties` files are also checked: *however, astyle does not work on `.properties` and `.xml` files*. So you will have to find other ways to get those files to comply with the Google checks, such as using the automatic formatting plugins  built into editors such as VSCode.

After using astyle to make this transformation, running `mvn checkstyle:check` with the options above  reports no violations:

```
% mvn checkstyle:check
...
[INFO] --- maven-checkstyle-plugin:3.1.1:check (default-cli) @ demo-spring-google-oauth-app ---
[INFO] Starting audit...
Audit done.
[INFO] You have 0 Checkstyle violations.
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
...
% 
```

The next step would be to add in additional rules, one by one, from the [`google_checks.xml`](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml), until you have reached
full compliance with the Google spec.

# Making Exceptions for specific rule violations (by line of code)

Occasionally, you may have a good reason for violating one of the Google style rules.

You can indicate that specific lines of code in specific files should be exempted from specific
rules by using the `SupressionFilter` module, as shown here:

```
   <!-- https://checkstyle.org/config_filters.html#SuppressionFilter -->
   <module name="SuppressionFilter">
        <property name="file" value="checkstyle/suppressions.xml" />
    </module>
 ```
    
This specifies that if the file `checkstyle/supressions.xml` exists, it contains a list of the exceptions
to the rules you are willing to permit.

As an example, suppose you got these errors when running `mvn checkstyle:check`:

```
[INFO] Starting audit...
[ERROR] /Users/pconrad/github/ucsb-cs48-s20/project-idea-reviewer/src/main/java/edu/ucsb/cs48/s20/demo/Application.java:9:1: Line contains a tab character. [FileTabCharacter]
[ERROR] /Users/pconrad/github/ucsb-cs48-s20/project-idea-reviewer/src/main/java/edu/ucsb/cs48/s20/demo/Application.java:10:1: Line contains a tab character. [FileTabCharacter]
[ERROR] /Users/pconrad/github/ucsb-cs48-s20/project-idea-reviewer/src/main/java/edu/ucsb/cs48/s20/demo/Application.java:11:1: Line contains a tab character. [FileTabCharacter]
[ERROR] /Users/pconrad/github/ucsb-cs48-s20/project-idea-reviewer/src/main/java/edu/ucsb/cs48/s20/demo/Application.java:12:1: Line contains a tab character. [FileTabCharacter]
[ERROR] /Users/pconrad/github/ucsb-cs48-s20/project-idea-reviewer/src/main/java/edu/ucsb/cs48/s20/demo/Application.java:22:1: Line contains a tab character. [FileTabCharacter]
Audit done
```

Suppose that for some reason, you *really, really* wanted those particular tab characters to stay just as they are,
and *not* be reported as a style violation.

Here is an example of what you might put into `checkstyle/supressions.xml` to indicate this:

```
<?xml version="1.0"?>
 
<!DOCTYPE suppressions PUBLIC
     "-//Checkstyle//DTD SuppressionFilter Configuration 1.0//EN"
     "https://checkstyle.org/dtds/suppressions_1_0.dtd">
 
<suppressions>
  <suppress checks="FileTabCharacter"
             files="Application.java"
             lines="9-12,22"/>
</suppressions>
```

# Making blanket exceptions for filename patterns

You can also specify exceptions to the rules by filename pattern.

For example, the sample `google_check.xml` includes these lines
which use the `[BeforeExecutionFileFilter`](https://checkstyle.org/config_filefilters.html#BeforeExecutionExclusionFileFilter) to indicate
that the rules should be ignored for any file with a name that ends in `module-info.java`

Note that the `fileNamePattern` parameter is a regular expression, hence the need for the `\`
characters in front of `-` and `.` and the need for the `$` to indicate the end of the expression.

```
   <!-- Excludes all 'module-info.java' files              -->
   <!-- See https://checkstyle.org/config_filefilters.html -->
   <module name="BeforeExecutionExclusionFileFilter">
        <property name="fileNamePattern" value="module\-info\.java$"/>
   </module>
```

# Another options for putting code in Google Style

Another option for addressing code style issues is 
* <https://github.com/google/google-java-format>

Note that this option is not configurable:
> Note: There is no configurability as to the formatter's algorithm for formatting. This is a deliberate design decision to unify our code formatting on a single format.
