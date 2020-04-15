---
topic: "Node: Windows"
desc: "Installing and working with Node, npm, nvm on Windows"
indent: true
category_prefix: "Node: "
---

# Windows (plain)

You can download an installer for node for Windows from here: <https://nodejs.org/en/download/> 


# Windows Subsystem for Linux (WSL)

The linked page from Microsoft has information on installing node for WSL.
* It recommends installing `nvm` (the Node Version Manager) first.
* It includes the entire process of enabling WSL and installing a distribution, so if you are already past all that, skip down to the part that 
* It is important to remove all previous version of node first.  This command seems to work:
  ```
  sudo apt-get purge --auto-remove nodejs
  ```
  Source: <https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04>

Link: <https://docs.microsoft.com/en-us/windows/nodejs/setup-on-wsl2>
