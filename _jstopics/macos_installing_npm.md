---
topic: "MacOS: Installing node"
desc: "Installing what you need for working with node/next.js/React on MacOS (e.g. npm command)"
category_prefix: "MacOS: "
---

To work with the Next.js stack, you'll need to install node, which installs the `npm` command.

One of the easiest ways to do this on MacOS is via the brew package manager.

# Installing `brew`

* To install `brew`, visit <https://brew.sh> and follow the instructions
* Before installing anything with `brew`, it's good practice to run `brew update` first.
* If your `brew` install is messed up, `brew doctor` can help restore a proper install.

# Using `brew` to install node

This command will install `node` using `brew`

```
brew install node
```

To test if it worked, you can try:

```
npm --version
```

