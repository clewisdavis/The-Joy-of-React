# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Lazy Loading and Streaming SSR

What is the relationship between 'lazy loading' and 'streaming SSR' stuff?

- They are both similar, improving the initial load experience by breaking it up into chunks, they solve different problems. And generally cannot be used interchangeably. **They are separate tools that are used in different situations.**

- Streaming SSR us used when our HTML rendering is blocked by some asynchronous work. For example, on a product page, you cannot render the 'shirt category' page until we have the list of shirts from the database. The server is blocked, and we unblock it by giving it permissions to send the HTML in chunks.

- Lazy loading, by contrast does not affect server side rendering, at all. It is explicitly about how our client side JS is split into different bundles.

Here is a good way to think about it:

- We use Streaming SSR wen the server is taking too long to generate the HTML.
- We use lazy loading when the client is taking too long to download the JS.

- Lazy Loading is a little less aggressive version of React Server Components. With lazy loading, we send the JS later, but with Server Components, we don't sen the JS at all.

- 'Suspense' is a low level tool that is used in all of these situations to define boundaries around logical chunks or our app.

## In summary

Good to note, that 'lazy loading' is a much broader term. For example, we can set a native HTML `<img>` tags to lazy load:

```JAVASCRIPT
<img
  src="/animals/panda.jpg"
  alt="an adorable panda chewing on bamboo"
  loading="lazy"
/>
```

- This is typically added to images that are 'below the fold', outside the viewport. The browser will defer loading the image until it's about to be scrolled into view.

- The same fundamental idea, don't load the thing until the user needs it!

- `React.lazy()` is a handy tool to keep in back pocket, especially when working with larger NPM packages. Not something use everyday, since Next is already heavily optimized out of the box.
