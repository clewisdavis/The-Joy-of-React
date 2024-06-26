# The Joy of React - Module 6 - Course Notes

- [Course Outline Notes](course-notes.md)

## Introduction, Full Stack React

So far, we have been dealing with React exclusively on the client. In this module, we are going to look at building full stack apps with React.

This course, developed in summer 2023, new advanced things to are ready to use. Suspense, React Server Components.

The only way to take advantage of al the new stuff, is to use Next.js

Next.js, is a full stack React meta-framework created and maintained by Vercel. Originally released in 2016, and brought common things like file-based routing.

So many things open up when you can introduce a server into the mix.

### Client vs. Server Rendering

- [Client and Server Side Rendering](./module-6-full-stack/01-notes-client-server-rendering.md)

### React Server Components

- [React Server Components](./module-6-full-stack/02-notes-server-components.md)

### Intro to Next

- [Intro to Next.js](./module-6-full-stack/03a-notes-intro-next-SSR-gatcha.md)
- [Next SSR Gotchas](./module-6-full-stack/03a-notes-intro-next-SSR-gatcha.md)
- [Next SSR Exercises](./module-6-full-stack/03b-notes-intro-next-SSR-exercises.md)

### Routing

- [Routing](module-6-full-stack-routing/01-notes-routing.md)

### Next's Metadata API

- Page is missing a 'title' tag, or any SEO attributes, as part of the documents `<head>`
- The `metadata` object let's you define a 'title' tag and any SEO attributes you want to add to your 'HTML' document.

- You can hardcode this, in your 'layout.js' file, and just manually add a `<head><title>Hello</title></head>` tag.

```JAVASCRIPT
function FlashMsgLayout({ children }) {
  return (
    <html lang="en">
      <head>
        <title>This is the title! 😄</title>
      </head>
      <body>
        <ToastProvider>
          {children}
          
        </ToastProvider>
      </body>
    </html>
  );
}
```

- This works ok, but a better way to do this in Next.js, is to use the MetaData API. You call the `metadata` api, and it's an object that defines metadata about the page.

```JAVASCRIPT
export const metadata = {
    title: 'Title Goes here!',
}
```

- All kinds of stuff you can put in this 'metadata' object, SEO tags, favicon, etc.
- For favicon.io, you can drop that asset directly in the 'app' directory, and it will be used.

```JAVASCRIPT
export const metadata = {
  title: '😄 👩‍🚀 Website!',
};

function FlashMsgLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <ToastProvider>
          {children}
          
        </ToastProvider>
      </body>
    </html>
  );
}
```

- Allowed to have meta data in any 'layout.js' file or 'page.js' file.
- This will work on most cases, but what if you want a title, programmatically?

- Next.js has an escape hatch for this, instead of defining a 'metadata' object, you return a function.

```JAVASCRIPT
export function generatedMetadata() {
    return {
        title: 'Generated title',
    }
}
```

- 🆒 Neat thing about this, Next will call this function in the same way it calls the other functions in your component, so you have access any params/data to display in your title. A profile name for example.

- In this case, the page title depends on some data we are using inside the component.

```JAVASCRIPT
export async function generatedMetadata({ params }) {

    const profile = await getProfileInfo(params.profileId);
    return {
        title: `${profile.name}'s profile`,
    };
}
```

### Creating, Building and Deploying

In this section, going to look into everything needed to start a new project, generating files and publishing your app on the internet.

- Bootstrapping new projects with `create-next-app`
- Using import aliases to get rid of all those `../`'s
- Generating a production build, and how to run it locally
- Analyzing our bundles, to understand what they consist of
- Deploying our apps, to our own custom domain
- Automatically generating preview build for work-in-progress branches

- [Starting a new project](./module-6-deploying/01-notes-starting-project.md)
- [Import Aliases](./module-6-deploying/02-notes-import-alias.md)
- [Building for Production](./module-6-deploying/03-notes-production.md)
- [Deploying](./module-6-deploying/04-notes-deploying.md)

### Rendering Strategies

Remember in an earlier lesson, talked about several different flavors of SSR, three different strategies:

1. 'On demand', SSR, where the HTML is generated per-request
2. 'Static Site Generation', where the HTML is generated for every route when the app is built
3. 'Incremental Static Regeneration', where each route is rendered on-demand for the first request, but the HTML is saved for future requests

- Next will use different strategies depending on the route.
- In the example from an earlier exercise, 'server timestamp', everything works fine in 'development' because it uses the SSR dynamically, on-demand.
- But when we deploy it, it uses a 'production' build.

- When we build our app, Next tries to figure out the most optimal strategy for each route.
- The 'default' behavior is to use a 'static' strategy, since it's the most performant.
- Next will flip to a 'dynamic' strategy if we are diong something that can't be calculated during the build (trying to read cookies for the request, accessing request headers etc. )

To summarize:

- In development, Next always uses a 'dynamic' on demand SSR strategy
- In production, Next will try to pick the optimal strategy on a route by route basis
- The 'default' strategy is 'static', pre-generating al the HTMl during the build process
- Using certain Next APIs will cause Next to switch to a 'dynamic' strategy for a particular route. For example, if we try and access cookies, Next knows it needs to do teh SSR dynamically, for every request
- Because our 'Server timestamp' example doesn't use any of these APIs, Next cannot tell that we want it to be done dynamically.

The good news is, we can explicitly tell Next which strategy to use.

- Switching Rendering Strategies
- Dynamic Segments

- [Rendering Strategies](module-6-rendering-strategies/01-notes-rendering-strategies.md)

### React Cache

This is like `react.memo` but for functions rather than components. This is what it looks like...

```JAVASCRIPT
import React from 'react';

export const getProfileInfo = React.cache(
  async (profileId) => {
    await delay(Math.random() * 200 + 400);
    return DATA[profileId];
  }
);
```

- `React.cache` allows us to memoize a function, so that the work won't be repeated when the function is called multiple times with the same parameters.

🟨 This is not an official part of React.

- Video Explanation - [React Cache](https://courses.joshwcomeau.com/joy-of-react/06-full-stack-react/08-react-cache)

### Suspense

- [Suspense, New World, Graphing it Out](module-6-suspense/01-notes-suspense.md)
- [Understanding Suspense](module-6-suspense/02-notes-understanding.md)
- [Putting It All Together](module-6-suspense/03-notes-putting-together.md)
- [Using Suspense in Next](module-6-suspense/04-notes-using-next.md)
- [Suspense Exercises](module-6-suspense/05-notes-suspense-exercises.md)

### Lazy Loading

Let's take an example, a library for rendering 'LaTeX', pronounced, 'lay tek', in some regions.

- If you were to use the entire package, you would be adding '72 gzipped kilobytes' to the client!

```JAVASCRIPT
import 'katex/dist/katex.min.css';
import Latex from 'react-latex-next';

function App() {
  return <Latex>{'$\\sqrt{x^2+1}$'}</Latex>;
}

export default App;
```

![laytek](images/image-38.png)

#### Understanding lazy loading

- [Understanding Lazy Loading](module-6-lazy-loading/01-notes-understanding.md)

#### Lazy Loading and Server Side Rendering

- [Understanding Lazy Loading](module-6-lazy-loading/01-notes-understanding.md)
- [Lazy Loading and SSR](module-6-lazy-loading/02-notes-serverside-rendering.md)
- [Lazy Loading and Streaming SSR](module-6-lazy-loading/03-notes-lazy-loading-streaming-ssr.md)
- [Lazy Loading in Next](module-6-lazy-loading/04-notes-lazy-loading-next.md)
- [Baked in Laziness](module-6-lazy-loading/05-notes-baked-in-laziness.md)

#### Dark Mode

- [Dark Mode](module-6-dark-mode/01-notes-dark-mode.md)
