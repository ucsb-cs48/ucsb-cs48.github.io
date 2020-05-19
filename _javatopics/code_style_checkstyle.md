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
	
The file `sun_checks.xml` is one that's built into the `checkstyle` product, and is documented here: <https://checkstyle.sourceforge.io/sun_style.html>.  Another option is `google_style.xml` documented here: <https://checkstyle.sourceforge.io/google_style.html>.
