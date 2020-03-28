---
topic: "MacOS"
desc: "Setting up an environment to do CS56 work on your own Mac (not ssh'ing into CSIL)"
category_prefix: "MacOS: "
---

For certain kinds of programs, i.e. graphics, and especially sound, 
it may be more convenient to work directly on your Mac rather than 
ssh'ing into CSIL.

For audio or sound programs, this is especially true.

What do you need to install on your Mac to be able to do this?

* The JDK
* `ant` and/or `mvn` (Maven)

Instructions follow below.   Note that installing [MacOS: Homebrew](/topics/macos_homebrew/) may make some of this easier.

# Install the JDK

To see whether you already have the JDK installed, do this in a terminal window:

```
169-231-88-206:~ pconrad$ javac -version
javac 1.8.0_31
169-231-88-206:~ pconrad$ 
```
If you see some version of Java 1.8.something... then you are good to go.

If not, download and install a version for Mac OS from here:

<http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>

# Installing OpenJDK 11 on Mac

This article tells you how to deal with having multiple versions of Java on your Mac, and what to do with the "tarball" file that you might find when you download OpenJDK 11 for Mac, since there are no obvious installation instructions.  Unfortunately, the link to the tarball takes you to a page where the distribution is NO LONGER AVAILABLE.  More on that below.
* <https://dzone.com/articles/installing-openjdk-11-on-macos>

So if the link in that article does not have OpenJDK 11 for Mac, who does?

This worked for me.  Note that it requires [brew](https://ucsb-cs56.github.io/topics/macos_homewbrew/), a package manager for MacOS.
* <https://installvirtual.com/install-openjdk-11-mac-using-brew/>

Here's the short version:

```
brew update
brew tap AdoptOpenJDK/openjdk
brew cask install adoptopenjdk11
```

To check if you now have Java 11, open a new Terminal window and do:

```
java -version
```

If it worked, you should see something like this:

```
# java -version
openjdk version "11.0.2" 2019-01-15
OpenJDK Runtime Environment AdoptOpenJDK (build 11.0.2+9)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 11.0.2+9, mixed mode)
```

Note: after installing Java 11, you may want to do:

```
brew install maven
```

Or if you already have Maven installed:

```
brew upgrade maven
```

And then do:

```
mvn --version
```

To make sure that Maven is using Java 11 and not still using Java 8 or earlier.

# What if I Maven reports it is using Java 13

I recently got a new Mac and followed the instructions above.  I ended up with Java 11 as my default java compiled:

```
pconrad@Phillips-MacBook-Pro ~ % javac --version
javac 11.0.6
pconrad@Phillips-MacBook-Pro ~ % 
```

But I was frustrated the Maven was reporting that it was using Java 13:

```
pconrad@Phillips-MacBook-Pro ~ % mvn --version
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: /usr/local/Cellar/maven/3.6.3_1/libexec
Java version: 13.0.2, vendor: N/A, runtim: /usr/local/Cellar/openjdk/13.0.2+8_2/libexec/openjdk.jdk/Contents/Home
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.15.1", arch: "x86_64", family: "mac"
pconrad@Phillips-MacBook-Pro ~ % 
```

So I went back and looked at the messages during the `brew install maven`, and saw this:

```
pconrad@169-231-117-34 ~ % brew install maven
==> Installing dependencies for maven: openjdk
==> Installing maven dependency: openjdk
==> Downloading https://homebrew.bintray.com/bottles/openjdk-13.0.2+8_2.catalina
==> Downloading from https://akamai.bintray.com/65/65adca036393f528e3830cab8b0aa
######################################################################## 100.0%
==> Pouring openjdk-13.0.2+8_2.catalina.bottle.tar.gz
==> Caveats
For the system Java wrappers to find this JDK, symlink it with
  sudo ln -sfn /usr/local/opt/openjdk/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk

openjdk is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have openjdk first in your PATH run:
  echo 'export PATH="/usr/local/opt/openjdk/bin:$PATH"' >> ~/.zshrc

For compilers to find openjdk you may need to set:
  export CPPFLAGS="-I/usr/local/opt/openjdk/include"

==> Summary
🍺  /usr/local/Cellar/openjdk/13.0.2+8_2: 631 files, 314.6MB
==> Installing maven
==> Downloading https://www.apache.org/dyn/closer.cgi?path=maven/maven-3/3.6.3/b
==> Downloading from http://apache.spinellicreations.com/maven/maven-3/3.6.3/bin
######################################################################## 100.0%
🍺  /usr/local/Cellar/maven/3.6.3_1: 87 files, 10.7MB, built in 8 seconds
==> Caveats
==> openjdk
For the system Java wrappers to find this JDK, symlink it with
  sudo ln -sfn /usr/local/opt/openjdk/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk

openjdk is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have openjdk first in your PATH run:
  echo 'export PATH="/usr/local/opt/openjdk/bin:$PATH"' >> ~/.zshrc

For compilers to find openjdk you may need to set:
  export CPPFLAGS="-I/usr/local/opt/openjdk/include"

pconrad@169-231-117-34 ~ %
```

It appears that Maven brings in Java 13 as a dependency.  Not ideal if we are trying to target Java 11.

However, my hope is that if we configure our `pom.xml` files to use Java 11, perhaps this won't be an issue.

I will also see if I can figure out a way to get Maven to actually point to the Java 11 software as its default Java implementation.

# Install Apache Ant

If you need to install Apache Ant for any reason, first check to see if you already have it:

```
169-231-88-206:~ pconrad$ ant -version
Apache Ant(TM) version 1.9.7 compiled on April 9 2016
169-231-88-206:~ pconrad$
```

If instead, you get `command not found`, then install Apache Ant in one of the following ways:

* `brew install ant`
   * This requires Homebrew, which is described here: [/topics/homebrew](/topics/homebrew)
* These instructions: <https://www.mkyong.com/ant/how-to-apache-ant-on-mac-os-x/>
   * A student in W20 reported that these didn't work well, and that `brew install ant` worked better.

