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
| []() | []() |
| []() | []() |
| []() | []() |
{:.table .table-sm .table-striped .table-bordered}


# Components, Stories, Knobs

Storybook has many features, but the three we will focus on are:
* Components: You start working with Storybook by adding a React Component to the Storybook.  
  - You add a component to the story book by adding a source files in the appropriate spot.
  - "The appropriate spot" depends on how you have your project configured.
  - In the example storybooks we are providing above, the "appropriate spot" is under `stories`, in a file called
    `ComponentName.story.js`.
  - That is a consequence of this line of code in `.storybook/main.js`:
    ```
    module.exports = {
      stories: ["../stories/**/*.stories.js"],
    ...
    ```
* Stories: Stories are the individual items defined inside a `ComponentName.stories.js` file.
* Knobs: Knobs are controls that allow you to interact with a component to simulate different parameters values being
  passed in for its `props`.
    
