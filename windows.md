---
topic: Windows
desc: "Setting up an environment to do CS56 work on your own Windows machine (not ssh'ing into CSIL)"
---

For certain kinds of programs, i.e. graphics, and especially sound, 
it may be more convenient to work directly on your Window machine rather than 
ssh'ing into CSIL.

For audio or sound programs, this is especially true.

What do you need to install on your Windows to be able to do this?

There are 2 options for working in a Windows environment.
  * "Natively" - directly in the Windows OS and filesystem 
  * On the Windows Subsystem for Linux (WSL) - a tool that basically creates a separate Linux environment alongside your Windows environment. Kind of like a more lightweight VM, with access to local storage.
    * Any files you create within the WSL are still stored in your local filesystem, and can be found by entering `\\wsl$` into the filepath bar in a File Explorer window
    
## Getting started with WSL

If you don't have the WSL activated on your computer already, follow the directions at the following link to set it up. There will be a variety of Linux distributions from which to choose. If you don't have a preference, choose Ubuntu. It is the most popular, so online documentation and discussion about the WSL usually pertains to the Ubuntu distro.
    * https://docs.microsoft.com/en-us/windows/wsl/install-win10
    
Once you have the WSL set up, you should see the corresponding app (Ubuntu, for example) in your Start Menu. Opening the app will launch a terminal instance in that environment. You can navigate using all the typical Unix commands.

### Java on WSL

Run the following commands to get your Java environment set up:

* To install the Java JDK <br />
`sudo apt-get update` <br />
`sudo apt-get install default-jdk` <br />
 To verify Java JDK has been installed <br />
 `java -version`

* To install Ant <br />
`sudo apt install ant` <br />
To check  <br />
`ant -version`

* To install Maven<br />
`sudo apt install maven`<br />
To check <br />
`mvn -version`

**Make sure you have Java-11, not an earlier version!**

### Git on WSL

WSL should come with git, but you can check by running `git --version`. If it is not installed, then follow the instructions at this link: https://peteoshea.co.uk/setup-git-in-wsl/

If you want to work remotely via SSH, you will have to generate new SSH keys specific to the WSL environment. For instructions on how to do that, take a look at this page: https://ucsb-cs56.github.io/topics/github_ssh_keys/
   * You don't need to set up SSH keys, since you can always work remotely with repos via HTTPS, but using SSH keys just makes it easier since you will not have to re-enter your GitHub login information whenever you want to clone a repo or push/pull.

### Heroku on WSL

The Heroku CLI is normally installed with snap, but the WSL doesn't currently support snap so you won't be able to install it with the commands that would be used in a normal Linux environment. Instead, run `curl https://cli-assets.heroku.com/install.sh | sh` to install the CLI.

### WSL with VSCode

If you are currently a VS Code user (or considering becoming one), you can install an extension to be able to access/edit/track files in the WSL from VS Code. Follow the instructions [here](LINK) to get started.

# Install git for Windows

* Download link: <https://git-scm.com/download/win>

(Note: I recommend git for windows, not "github for windows".  "git for windows" provides the "Git Bash Shell" as well as the command line git tools that we use in CMPSC 56.)

# Install the JDK for Java 11

As of W20, we do not have current "tested" instructions for installing the Java 11 JDK on Windows. As/when that changes, we'll update this page.

We strongly encourage you not to install a later version than Java 11.  Java 11 is a "long-term support" version of Java, while versions such as 12, 13, etc. are considered more experimental.

# Install Apache Ant

As of W20, we do not have current "tested" instructions for installing Apache Ant on Windows. As/when that changes, we'll update this page.

# Install Apache Maven

As of W20, we do not have current "tested" instructions for installing Apache Maven on Windows. As/when that changes, we'll update this page.

