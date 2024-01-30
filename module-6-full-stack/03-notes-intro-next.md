# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Intro to Next.js

Next.js, known as Next, is a popular full stack React meta-framework, as of mid-2023. And the only framework to take advantage of React's new full stack model, including React Server Components (RSC).

📔 Two different Next js

- Important to know about Next is that it has changed dramatically this year.
- In May, 2023, Next released 'v13.4', which is the official stable release of the 'App Router'.
- A complete re-write of Next.js, done to integrate all teh new React features like RSC.

How can you tell is something is out of date, using an older version?

- A directory structure that involves `/pages` directory.
- Methods named `getServerSideProps` and `getStaticProps`
- Files named `_app.js` and `_document.js`
- API Routes

### Hello Next

Running a local Next server, and the files required. Have standard 'package.json' that holds your dependencies and your npm commands for building.

- In the 'src' folder, this holds are our source code. And inside that, we have our 'app' directory. `src > app`.
- The `app` directory is new, and this is new for Next 13, and this is the home for all of our routes.

```CODE
package.json
--src
    --app
```

- Within the app directory, have a style sheet, with some default styles, 'styles.css'
- And then we have two separate components.
  - Inside of 'page.js', a `Home` component.
  - And a `RootLayout` component inside of 'layout.js'
- 🤔 Next uses a **file based routing system**, 📣 which means I have to name my file, 'page.js' if you want it to be publicly viewable 📣.
- When you put a 'page.js' file right inside the 'app' directory, not nested inside any sub-directories, **this becomes the component that is rendered on the homepage**. Your root directory 'localhost:3000'.

- In Next.js, pages cannot be used on their own, they have to use a layout.
