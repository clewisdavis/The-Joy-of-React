# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Intro to Next.js / SSR Gotchas

Get into one of the biggest gotchas for server side rendering. Earlier in the course, we saw how we [can persist state in localStorage](https://courses.joshwcomeau.com/joy-of-react/03-hooks/05.02-initial-effect-exercises), for example:

```JAVASCRIPT
'use client';

function Counter() {
  const [count, setCount] = React.useState(() => {
    return Number(
      window.localStorage.getItem('saved-count') ||
      0
    );
  });

  React.useEffect(() => {
    window.localStorage.setItem('saved-count', count);
  }, [count]);

  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

This works fine in a client side rendering app (Parcel), but blows up when we do this sort of thing in a Client Component in Next, or any other SSR framework.

See the problem? Hint, 'ReferenceError: window is not defined'.

- In order to initialize the `count` state variable during the first render, we run the following code:

```JAVASCRIPT
window.localStorage.getItem('saved-count');
```

Here is the problem: There is no `window` object on the server.

- Think about it, on the first render, server-side-render a React component, that first render happens in a 'headless' environment. In Node.js, there is no browser window. There is no DOM.
- `window.localStorage` is designed to read/write on teh user's device. The Node.js server cannot know which value is saved locally on the user's phone or computer.

- As a result, Node.js will not be able to complete the server-side render, and it will serve up a blank HTML file.
- This is one of two very common pitfalls with SSR.

1. Trying to access 'browser stuff' on the server.
2. Hydration mismatches.

Get some practice, with this repo: [next-13-ssr-gatchas](https://github.com/clewisdavis/next-13-ssr-gotchas)
