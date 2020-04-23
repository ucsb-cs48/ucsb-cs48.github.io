---
topic: "localhost"
desc: "What does it mean to run a web server on localhost?"
---

When we are developing a web application using a technology such as node, Spring Boot, Python Flask, or Rails, we
often run our web server on `localhost` rather than on an internet facing web server.

This article explains:
* explains what `localhost` means
* describes some common problems that arise, and how they can be overcome.

# What does "running on `localhost`" mean?

Running on `localhost` means that 
* we are running the server on our local machine
* it can *only* be accessed from web browsers running on that local machine

Running on localhost is convenient for testing code as we develop; it is not the way we make the web app available to real users.

The details depend on the framework, but the following table shows what is typical.  Note that the numbers shown, such as `8080`, `3000`
are typical defaults, but can be set to other numbers by the program code.

| Framework | Command to start `localhost` server | Where you typically point your web browser |
|-----------|-------------------------------------|----------------------------------|
| Spring Boot (Java) | `mvn spring-boot:run` | <http://localhost:8080> |
| next.js (JavaScript) | `npm run dev` | <http://localhost:3000> |
| Rails (Ruby) | `rails s` | <http://localhost:3000> |

Note that while `localhost` means the local machine, context matters.  

* If you are running the server directly on your laptop or desktop machine, than that laptop/desktop machine itself is `localhost`
* But, if you are ssh'd into a CSIL host, then the CSIL host you are ssh'd into (e.g. csil-04.cs.ucsb.edu) is actually the `localhost` machine.

(Also, to be pedantic, `localhost` actully refers to a *network interface* of the local machine (host), rather than the 
machine (host) itself, but
that's  a detail that you can explore if/when you take CMPSC&nbsp;176A, the upper division networking course.) 


## About `localhost` and "Port Numbers"

The code for your app is typically configured to run on a particular port number, e.g. `8080` or `3000` by default.

The port number is a more specific "communications channel" on a particular network host. 
You can find more information on port numbers
at this short article, which you are encouraged to read if you are not already familiar with port numbers
(or, for that matter, even if you are): <https://ucsb-cs56.github.io/topics/port_numbers/>

The port number is the number that comes after the `:` in a web address such as <http://localhost:8080> or <http://localhost:3000>.
It is needed because, by default, web servers run on port `80`, but development servers typicaly are *not* given
permission to run on port `80`.  (The reason for that is beyond the scope of this discussion).

## What if I get `port already in use`

The error `port already in use` signifies that someone else on the same system (perhaps you, in another window?) 
is already using the port you are trying to use.

If you are running on your own laptop/desktop, this almost always is because you either have:
* another window open already running the server
* a stray process that is running the server

If it's another window, that's straightforward; just use CTRL/C to shut down that other server.

If it's a stray process, you either need to use your OS's equivalent of "Task Manager" or "kill -9" to shut down the
rogue process.  (Or, failing that, the nuclear option is to just reboot your system).

The final case is that you might be running on a CSIL or CSTL box where someone else is also ssh'd in and running
on that port.    In this case you can either try running on a different machine, or you can try to redefine the port
being used.

The way to redefining the port being used depends on the specific technology:

A few examples:

* For Spring Boot applications, you may be able to simply do, for example:
  `export PORT=8082` before running `mvn spring-boot:run`
* For next.js application
  - in `package.json`, under "scripts" for the key "start" you need "next start -p $PORT"
  - add `PORT=3002` to `.env`
  
## How do I access `http://localhost:8080` on CSIL from my laptop?

Suppose you are running your Spring Boot application on `localhost:8080` on one of the CSIL 
machines.

You normally *will not* be able to access that application from any browser that is NOT directly
running on that CSIL machine.

If you are ssh'ing into CSIL on your laptop (e.g. using `ssh` in a terminal session, or using an app such as `PuTTY` or `MobaXTerm` on Windows) keep in mind that if you direct your browser (running on your laptop) to `localhost:8080`, that request *never leaves your laptop*.  It looks for a web app running *on your laptop*.

# Using ssh tunnelling

To solve this you can use *ssh tunneling*.  Here's how it works.

Suppose you are running on port `8080` on host `csil-10.cs.ucsb.edu`

Then you'll type the command: 

```
ssh -L 12345:localhost:8080 username@csil-10.cs.ucsb.edu
```

* If you have Mac or Linux, this should "just work".   
* On Windows, you may need to install [Git For Windows](https://git-scm.com/download/win) and use the `Git Bash Shell` to do this.

What this does is make it so that when you navigate to `http://localhost:12345` on your local browser, 
it sends the web request and response through an "SSH Tunnel" to port `8080` on `csil-10.cs.ucsb.edu`

If you are using OAuth, it may be better to use the name number (e.g. 8080) on both sides instead of `12345`, e.g. 

```
ssh -L 8080:localhost:8080 username@csil-10.cs.ucsb.edu
```

This redirects `localhost:8080` on your local browser to `localhost:8080` on the remote machine.
