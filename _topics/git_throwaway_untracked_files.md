---
topic: "git: throwaway untracked files"
desc: "how to clean up untracked files easily"
indent: true
---

How to remove local untracked files from the current Git branch

* To remove directories, run `git clean -f -d` or `git clean -fd`
* To remove ignored files, run `git clean -f -X` or `git clean -fX`
* To remove ignored and non-ignored files, run `git clean -f -x` or `git clean -fx`

Source: <https://koukia.ca/how-to-remove-local-untracked-files-from-the-current-git-branch-571c6ce9b6b1>g
