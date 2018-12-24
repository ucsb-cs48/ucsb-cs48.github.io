---
topic: "CSIL: via ssh from Windows"
desc: "Connecting via PuTTY/XMing or MobaXterm"
indent: true
category_prefix: "CSIL: "
---

To connect to the CSIL machines from Windows, you need a piece of software known as an *ssh client*

# Goodbye PuTTY/XMing, Hello MobaXTerm

The most commonly used ssh client in the past has been a program called PuTTY.

PuTTY works fine for any program that doesn't use graphics.  However, to access the graphics capabilities of the 
CSIL machines, you also need a piece of software known as an *X11 server*, such as XMing.

Configuring PuTTY and XMing to work together can be tedious.   Why bother, when there is now a free program that
combines both, called MobaXTerm.

You can download MobaXterm from this link: [http://mobaxterm.mobatek.net/](http://mobaxterm.mobatek.net/)

# How to use MobaXTerm to connect to CSIL

Once you have downloaded it, you want to use it to create a new session.  The [demo shown at this link](http://mobaxterm.mobatek.net/demo.html) pretty much illustrates the process.  

* Click "new session"
* Select "ssh"
* For "remote host", instead of `192.168.56.86` you'll enter `csil-15.cs.ucsb.edu` (actually, instead of `15`, choose any number between 01 and 48).
* For "username", instead of `root`, you'll enter your CSIL username.
* The first time you connect to a particular system, you may be asked 
* It should then prompt you for your password.  (I do not recommend "saving the password".)

