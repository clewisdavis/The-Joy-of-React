# The Joy of React - Project

- [Course Outline Notes](course-notes.md)

## About this Project - Toast Component

- In this project we will build our very own 'Toast' component.

- Like so much in front end dev, Toast components are deceptively complex. If you want to build an acessible, production ready component, we have to be careful with our decisions.

### Intro to the Toast UI component

- A toast message is a common UI pattern used to deliver notifications. A toast message pops up, like a piece of toast popping out of a toaster.
- Semantically they are a bit like dialogs/modals, but with 2 big differences.

1. Dialogs should be used when immediate attention is required. They generally block the user from what they were doing, trapping focus within the dialog. Toast messages, by contrast, should be used for non-urgent updates. They should no interfere with what the user is doing.

2. Multiple toasts can be present at once, we are not limited to one at a time.

In our implementation, toasts will stack up in the bottom corner.

### Project Format

Like the 'Word Game' project, given a good chunk of the starter code, using Parcel. And the 6 exercises, you will finish implementing this component, adding new features and refactoring the code.

All the required CSS has been provided as well, but you can change it. Each exercise has hints and solution video.

#### Choose your own adventure

This is a hard one, especially if you have never worked in production React codebase. But you learn best by doing. But you can take advantage of the resources.

1. Use the 'hints' in each exercise to get unstuck
2. Solution videos on each on how to solve the problem
3. And use the community in Discord

Path's, if you have a good amount of React experience and high tolerance for frustration. Go this path.

- Give each exercise a solid attempt
- If get stuck, use the hints
- If that doesn't help, watch solution video
- If still confused, as questions in Discord

If new to React, and don't want to feel frustrated.

- Watch the solution videos first, to see how to solve the problem. Ask questions in Discord if something doesn't make sense.
- See if you can reproduce the solution, without checking the video.
- Consult the hints if you get stuck, and re-watch the solution video if hints don't help

### No third party components

We build a `Toast` component from scratch, we will not use a pre-built component form any unstyled component library.

- Why wouldn't we take advantage of these tools, we just learned about them?
- The deal is, to develop a core set of React skills, but you don't want to come overly dependent on them.

- There will be times when you won't be able to take advantage of a library. Maybe you have a specialized case, that a library doesn't cover, or a library you depended on becomes unmaintained and incompatible with the newer version of React.

- Ideally we should not depend on any component we could not build ourselves. It's ok to use a handful of third party components, but if they disappeared tomorrow, you should be able to build them yourself.

## Getting Started - README.md file

- To get started, fork this [Github](https://github.com/joy-of-react/project-toast) repository:

<https://github.com/joy-of-react/project-toast>

- Happy Thanksgiving 🦃
- The README.md is the home base for this project, to guide you through.

### Getting started

- This project is created with Parcel, and be run locally on your computer using Node.js and NPM.
- To review and get started with [running local development environment](https://courses.joshwcomeau.com/joy-of-react/project-wordle/03-dev-server), lesson.

```CODE
# Install dependencies:
npm install

# Run a development server:
npm run dev
```

- To create new components, you can use the helper script to create all the files and standard markup.

```CODE
# Create a new component:
npm run new-component [TheNewComponentName]
```

### Troubleshooting

If you run into strange errors when trying to run a development server, start by deleting the `.parcel-cache` director. This holds temporary automatically-generated files, and sometimes things get out of sync, and need to be deleted.

If still have errors, then share them in the Discord.
