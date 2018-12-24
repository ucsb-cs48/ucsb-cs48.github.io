---
topic: "git: feature branch workflow"
desc: "One branch per feature/issue/story"
indent: true
---

This article describes a more advanced workflow than the [Git: Basic Workflow](/topics/git_basic_workflow/) where all work is done on the master branch.

You should be completely comfortable with the [Git: Basic Workflow](/topics/git_basic_workflow/) before diving into this more advanced topic.

# A workflow based on feature branches

In most "real-world" uses of git/github (e.g. at software companies), rather than working on the `master` branch being the common case,
it is far more often the case that you *never, ever, ever work directly on master!!!*

Instead, each time you start a new story, issue, feature or bug fix, you start by creating a "feature branch" as an alternative to the `master` branch.

When you are finished with the story, feature, bug fix, etc. you would do a pull request from the feature branch
to the `master` branch.  This could be a pull request within the same repository, or it could be a pull request from one fork of a repo to another.

# Using this workflow 

You are encouraged to use the branch method to avoid accidentally
making changes to pending/open pull requests while working on other
issues. That might happen when you do all your work on master because
pull requests don't use a snapshot of the code from the time that you
made the pull request; instead, they point to the branch you are
trying to pull from. This means if you push more changes to a branch
after making a pull request from that branch, these changes will be
reflected in the pull request immediately. And the goal here is to
submit each issue you resolve in a separate pull request.

Ideally, you will **never have a pull request open from your master
branch**. Instead, each pull request will be from a feature
branch. That way, you can merge your feature branches into your own
fork's master branch (once you are ready to integrate those changes
into your application) at your leisure and it won't mess up your other
pull requests (to merge a branch into your master branch, make sure
you are on master and then call `git merge BRANCH_NAME`, but only do
this on your fork).

This will also show you how many teams develop software **in the real
world**. In these environments, you **never, ever, ever** make changes
directly to the master branch. Master is for **production-ready
code**. Devvelopment teams use branches to keep in-development
projects/pieces separate.

# How to

When beginning work on an issue, you should start clean from the
master version. `git status` will tell you which branch you are on. If
you are not on master, switch to master with `git checkout master`
(your changes on the other branch will not be lost; they will still be
there if you `git checkout` back to that branch). Make sure you are
up-to-date by calling `git pull`, then create a new branch with `git
checkout -b BRANCH_NAME`.

```
$ git checkout -b feature/text-color
Switched to a new branch 'feature/text-color'
$
```

The `-b` means to create a new branch; if you are switching to an existing branch, you can just leave off the `-b`. 

Moving forward, you will use:

* `git push origin feature/text-color` rather than `git push origin master`
* `git pull origin feature/text-color` rather than `git pull origin master`

Branch naming conventions vary slightly, but one way is to preface feature branches with "feature/" and bug-fixing branches with "hotfix/". For example, `feature/text-color` or `hotfix/empty-list-crash`.

After you've committed all your changes on this feature branch, push
it, and then open a new pull request to the master branch of the main repo
from the branch you were working on and include the issue in the pull
request description.

![cs56-mentor-screenshot-pullrequest-branches](https://cloud.githubusercontent.com/assets/6810760/20235019/fad8cc34-a83b-11e6-85ae-d02c89a79ce7.png)
