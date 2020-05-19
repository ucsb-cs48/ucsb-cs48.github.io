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

# A Strategy for deploying `checkstyle`

