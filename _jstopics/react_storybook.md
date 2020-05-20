---
topic: "React: Storybook"
desc: "A tool for cataloging, documenting and testing React components"
category_prefix: "React: "
indent: true
---

Storybook (documented at: <https://storybook.js.org/> is a tool for cataloging, documenting, and testing JavaScript UI components.  

It can be used with many frameworks, but for this page, we focus on its use with React.

Here are three examples of React Storybooks, along with the repos from which the UI components are taken

| Storybook | Repository | 
|-----------|------------|
| [project-idea-reviewer-nextjs-storybook](https://ucsb-cs48-s20.github.io/project-idea-reviewer-nextjs-storybook) | [project-idea-reviewer-nextjs](https://github.com/ucsb-cs48-s20/project-idea-reviewer-nextjs) |
| [cs48-s20-nextjs-tutorial-storybook](https://ucsb-cs48-s20.github.io/cs48-s20-nextjs-tutorial-storybook) | [cs48-s20-nextjs-tutorial](https://github.com/ucsb-cs48-s20/cs48-s20-nextjs-tutorial) |
{:.table .table-sm .table-striped .table-bordered}


# Components, Stories, Knobs

Storybook has many features, but the three we will focus on are:
* Components: You start working with Storybook by adding a React Component to the Storybook.  
  - You add a component to the story book by adding a source files in the appropriate spot.
  - "The appropriate spot" depends on how you have your project configured.
  - In the example storybooks we are providing above, the "appropriate spot" is under `stories`, in a file called
    `ComponentName.story.js`.
  - For example, if under the `components` directory you have files named `AppFooter.js` `AppNavbar.js` and `Layout.js`, then
    under `stories` you'd expect to see `AppFooter.stories.js`, `AppNavbar.stories.js`, and `Layout.js`.
  - That particular location and filename convention is a consequence of this line of code in `.storybook/main.js`:
    ```
    module.exports = {
      stories: ["../stories/**/*.stories.js"],
    ...
    ```
* Stories: Stories are the individual items defined inside a `ComponentName.stories.js` file.
* Knobs: Knobs are controls that allow you to interact with a component to simulate different parameters values being
  passed in for its `props`.
    
Here is an example from [project-idea-reviewer-nextjs](https://github.com/ucsb-cs48-s20/project-idea-reviewer-nextjs):

Consider the contents of `stories/AppNavbar.stories.js`:

```
import React from "react";
import { select, text } from "@storybook/addon-knobs";
import AppNavbar from "../components/AppNavbar";

export default {
  title: "AppNavbar",
  component: AppNavbar,
};

export const loggedOut = () => {
  return <AppNavbar />;
};

export const loggedIn = () => {
  const name = text("Name", "Phill Conrad");
  const role = select("Role", ["admin", "student", "guest"], "guest");
  const picture = text(
    "Image URL",
    "https://avatars3.githubusercontent.com/u/1119017"
  );
  const user = { name, role, picture };
  return <AppNavbar user={user} />;
};
```

In the first three lines, we import React itself, the "knobs" we are going to use (a `select` and a `text`) knob, and then
the component about which we are writing the story:

```
import React from "react";
import { select, text } from "@storybook/addon-knobs";
import AppNavbar from "../components/AppNavbar";
```

The next four lines simply give a title and name to our component; you typically just put the name of the component into the `title` and `component`:

```
export default {
  title: "AppNavbar",
  component: AppNavbar,
};
```

Then, two individual stories are defined:

* `export const loggedOut = ...` starts the first of the two stories
* `export const loggedIn = ...` starts the second of the two stories 


The first of these two stories does not make use of any knobs.  It is a story that shows what the navigation bar looks like when the user is logged out.  Since there is only one way the screen can look, there is no need for a knob.   The name of the function, `loggedOut` is the name of the story that will appear in the storybook.  All way have to do is reeturn the component:

```
export const loggedOut = () => {
  return <AppNavbar />;
};
```

The second story is a bit more complex.  When the users is logged in, there are a few things that can affect how the navigation bar looks: 
* The name of the user
* The role of the user (admin, student or guest)
* The url of an image for the user

Each of those is turned into a knob with this code.  This makes a UI element that sets the values to default values, but
also allows the user to override these values with other ones and see the effect on the React component:

```
const name = text("Name", "Phill Conrad");
  const role = select("Role", ["admin", "student", "guest"], "guest");
  const picture = text(
    "Image URL",
    "https://avatars3.githubusercontent.com/u/1119017"
  );
```

Then, these values are combined into a value for `user` that can be passsed into the user props of the component:
```
  const user = { name, role, picture };
  return <AppNavbar user={user} />;
```

That, in a nutshell, is how you write stories for React storybook.

The only other things you need to know are:
* How to get React Storybook configured in your repo
* How to run your Storybook on localhost
* How to get a version of your Storybook deployed on the web using GitHub pages

# How to get React Storybook configured in your project repo

This pull request can be used as a guide to adding Storybook support to your project:

* <https://github.com/ucsb-cs48-s20/cs48-s20-nextjs-tutorial/pull/8>


Here's a rundown of what needs to be done, (since some folks didn't seem to get it from reading the pull request.)

You need the file `.babelrc` with these contents:

```
{
  "presets": ["next/babel"]
}
```

You need the files `main.js` and `preview.js` under `.storybook` as in this repo:

* <https://github.com/ucsb-cs48-s20/cs48-s20-nextjs-tutorial/tree/master/.storybook>


You need to make several changes in your `package.json`.

Those include these lines under `scripts` (replace `cs48-s20-nextjs-tutorial-storybook` with the name
of the `-storybook` repo where you will publish your storybook on GitHub pages; because of the `../`,
this *must* be a sibling of your project repo.


```
    "storybook": "start-storybook -p 6006",
    "build-storybook": "build-storybook -c .storybook -o ../cs48-s20-nextjs-tutorial-storybook/docs"```
```

You also needs these lines in `devdependencies`:

```
    "@babel/core": "^7.9.6",
    "@storybook/addon-a11y": "^5.3.18",
    "@storybook/addon-actions": "^5.3.18",
    "@storybook/addon-knobs": "^5.3.18",
    "@storybook/addon-links": "^5.3.18",
    "@storybook/addon-storysource": "^5.3.18",
    "@storybook/addon-viewport": "^5.3.18",
    "@storybook/addons": "^5.3.18",
    "@storybook/react": "^5.3.18",
    "babel-loader": "^8.1.0",
    "terser-webpack-plugin": "^2.3.5"
```

Finally, you should also add the commands into the documentation in your README.md,
as was done in this PR:

* <https://github.com/ucsb-cs48-s20/cs48-s20-nextjs-tutorial/pull/8>

# How to run your Storybook on localhost

If you followed the steps listed above then you can run your storybook locally with:

```
npm run storybook
```

This should make the storybook available on <http://localhost:6006> as long as you keep that command running.

# How to get a version of your Storybook deployed on the web using GitHub pages

To make a version of your storybook that is deployed on GitHub pages:

One time setup:

1.  Create an empty repo that has the same name as your repo, but with `-storybook` at the end.  Clone this repo
    as a sibling of your repo (i.e. the directories where they are cloned have the same parent directory).
1.  Edit the `build-storybook` command in `package.json` so that it refers to your new empty repo.  For example:
    ```
        "build-storybook": "build-storybook -c .storybook -o ../project-idea-reviewer-nextjs-storybook/docs"
    ```
    
    The part you want to edit is the `../project-idea-reviewer-nextjs-storybook/docs` part.  The `project-idea-reviewer-nextjs-storybook` should be the name of the new empty repo you created.  The `..` part means to go to the parent directory, then down into the sibling directory.  The `/docs` part means that the documentation will be placed into the `/docs` subdirectory in the repo which is how it will get published to GitHub pages.
1.  Run `npm run build-storybook`
1.  `cd ../project-idea-reviewer-nextjs-storybook` (or whatever your repo is called)
1.  Use `git add .` then `git commit -m "publish storybook"`  then `git push origin master`
1.  Visit the settings for the `-storybook` repo, and scroll down to GitHub pages, and select `publish from master branch /docs`
1.  Visit the URL shown in the GitHub pages section for your repo, and see the published storybook.

Each time you want to update the repo:

1.  In the directory for your main repo, run `npm run build-storybook`
1.  `cd ../project-idea-reviewer-nextjs-storybook` (or whatever your repo is called)
1.  Use `git add .` then `git commit -m "publish storybook"`  then `git push origin master`
