---
topic: "git: .gitignore files"
desc: "What they are for and what to put in them"
indent: true
---

The `.gitignore` file is a hidden file in a Git repository that tells Git which files should NOT be committed to the repository.

Examples of files that should NOT be committed:

* Artifacts of compiling that can be obtained from source code, such as:
   * `.class` files in Java
   * `.jar` files produced from the source in the repo (as opposed to .jar files that are stored in the repo permanently
       because they are dependencies of the project)
   * dependencies that are automatically downloaded, e.g. by Maven
* Backup files or temporary files produced by editors, such as:
    * `*~` files such as `Foo.java~` produced by Emacs
    * `*.swp` files produced by vi/vim
* Artifacts of working on various Operating Systems
    * `.DS_Store` files on MacOS
    
   
Each language has its own set of file patterns, e.g.

* For C/C++, `*.o` 
* For Python, `*.pyc`

Some tools, too, have their own set of files that should be in the .gitignore.    Github has .gitignore files predefined for
frameworks such as Rails and Jekyll (which are both written in the programming language Ruby), the Unity game engine (folks typically
write C++ code for this), as well as the Maven version control system.

You can find examples of the files that should be in the .gitignore for various systems by doing a web search on ".gitignore name-of-system-or-tool".

The repo [https://github.com/github/gitignore](https://github.com/github/gitignore) also has a nice collection 
of these.

A few useful ones for this course:

* [Android](https://github.com/github/gitignore/blob/master/Android.gitignore)
* [Java](https://github.com/github/gitignore/blob/master/Java.gitignore)
* [Maven](https://github.com/github/gitignore/blob/master/Maven.gitignore)
