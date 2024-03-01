# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Starting a New Project

- The official way to create a Next app, use the `create-next-app` CLI utility.
- Distributed as an npm package, to install, `npm install -g create-next-app`, installs and used as an 'executable'.
- An easier way, to use the, `npx` utility. So `npx create-next-app`, will download the utility, execute it and cache it for next time.

- After running the command, the CLI will ask a series of questions to boostrap your app.

1. Project Name
2. Use Typescript, y/n
3. ESLint, y/n
4. Tailwind CSS, y/n, no will fallback to using CSS Modules
5. Use an 'src/' directory, y/n, yes
6. App Router, y/n, yes recommended, the new Next app router
7. Import Aliases, y/n

Will create all the files and you can open in VS Code. Then you can open it in VS Code, `code sample-next-app`, you will see the file structure.

- Finally to start the Next server, `npm run dev`
