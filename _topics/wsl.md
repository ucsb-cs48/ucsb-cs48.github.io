---
topic: Windows: WSL
desc: "Setting up the Windows Subsystem for Linux (WSL)"
indent: true
category_prefix: "Windows: "
---

For advanced users who are looking to have a full Linux command-line interface on their Windows machine, we recommend using Windows Subsystem for Linux (WSL). This will allow you to access package managers (such as `apt-get` for Ubuntu/Debian) and the full suite of UNIX commands.

The first step is ensuring that you have a compatible machine. You will need:
* Windows 10, build 16215 or later (but we recommend 18917 or later to use new [WSL 2 features](https://devblogs.microsoft.com/commandline/announcing-wsl-2/) and the new [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701)).
* Administrator privileges on your machine

The following guides from Microsoft detail the installation instructions for WSL and WSL 2 respectively. We recommend WSL 2 if your Windows 10 build supports it.
* WSL 1: <https://docs.microsoft.com/en-us/windows/wsl/install-win10>
* WSL 2: <https://docs.microsoft.com/en-us/windows/wsl/wsl2-install>

For users who want a nicer looking terminal, complete with tabs, emojis, and more customization features, you can optionally install the new [Windows Terminal (Preview) from the Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701).

Keep in mind that WSL uses UNIX line endings (LF) while Windows uses CRLF line endings. If you checked out your code natively in Windows (i.e. using Git Bash or GitHub Desktop), your Git repository may be using CRLF line endings, and therefore may cuase Shell scripts (and other programs that parse based on line endings) and Git commits to act differently or fail.
