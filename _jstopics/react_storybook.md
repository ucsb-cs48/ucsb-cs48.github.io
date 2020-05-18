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
| [teaching-practices-game-storybook](https://ucsb-cs48-s20.github.io/teaching-practices-game-storybook) | [teaching-practices-game](https://github.com/ucsb-cs48-s20/teaching-practices-game) |
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

This pull requests can be used as a guide to adding Storybook support to your project:

* <https://github.com/ucsb-cs48-s20/cs48-s20-nextjs-tutorial/pull/8>


# How to run your Storybook on localhost

TODO

# How to get a version of your Storybook deployed on the web using GitHub pages

TODO
