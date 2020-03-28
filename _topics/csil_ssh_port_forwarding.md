---
topic: "CSIL: ssh port forwarding"
desc: "How to access webapps running on CSIL from your local machine"
indent: true
---


When you want to access a `localhost:8080` web app running on CSIL from a non-CSIL computer, e.g. your laptop:

At a command prompt (terminal prompt on MacOS, Linux, WSL, Windows 10, or git bash shell on Windows), you can type this:

`ssh -L 8080:localhost:8080 username@csil-xx.cs.ucsb.edu`

where:
* `username` is your actual CSIL username
* `xx` is some number between `01` and `48`  

That will set up port 8080 on your local machine as a tunnel to "localhost:8080" on the CSIL machine.    Then, if you put `localhost:8080` in your browser, you should be getting access to `localhost:8080` on the CSIL machine you are ssh'ing into.
