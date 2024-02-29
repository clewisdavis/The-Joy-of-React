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
