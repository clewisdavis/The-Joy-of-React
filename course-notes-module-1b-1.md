# Module 1 - Components / Application Structure

- [Course Outline Notes](course-notes.md)

## Application Structure Lesson

### Application Structure

- Most React apps will share a common structure.

- An `index.js`, `App.js` and a `Header.js`
- See the [code sandbox](https://codesandbox.io/s/cehrev?file=/index.js&utm_medium=sandpack)

#### Index.js

- The root `index.js` file is the first bit of code to be executed. Responsible for rendering our React app, turning the elements we write into live DOM nodes.
- There will only be 1 spot int he entire codebase that calls the `createRoot` and `render` methods form react-dom.
- Common to do setup tasks in this file as well. Projects created with `create-react-app` include some performance, also you can include CSS files.
- Typically don't want to render a bunch of JSX, or include things like headers and buttons etc.
- General rule, just renders the single element: `<App />`

#### App

- Common for our projects to have a component called `App`
- This is the home base, React component in our project.
- Could manage core layout stuff, like headers and footers.
- If you are using a routing solution like react Router, top level routes are often included in this file.

#### Modules

- Generally use ES module system to split our apps into multiple files.
- For a refresher on JS modules

#### Going forward

- To keep things focused on the course, not showing `index.js`.

### Fragments

- Imagine you are writing the following HTML:

```HTML
<h1>Welcome to my homepage!</h1>
<p>Don't forget to sign the guestbook!</p>
```

- If we copy/paste that HTML into our React app, turning it into JSX, we get an error
- `Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>?`

- Why does this happen?
- 🤔 Hint: Try converting the JSX to `React.createElement` function calls.

- Why can't React simply return multiple elements?

```JAVASCRIPT
import React from 'react';

function App() {
    return (
        <h1>This is the heading</h1>
        <p>Welcome to this space!</p>
    );
}

export default App;
```

- In this format, it's not clear, but once you turn it into pure JS, it's more obvious.

```JAVASCRIPT
import React from 'react';

function App() {
  return (
    // works with just one
    React.createElement('h1', {}, 'Welcome')
    // fails with two
    React.createElement('p', {}, 'Sign in')
  );
}

export default App;
```

- In JS, the `return()` statement has one expression slot. You can return one expression, and it valid.
- **But you CANNOT add multiple expressions.**
- For example;

```JAVASCRIPT
let arr = [1, 2, 3];

return arr.push(4) arr.push(4) arr.push(4) arr.push(4)
```

- Looking at our React `App()`, imagine each element, `<h1>`, `<p>` as a function call.
- Then it makes more sense, you cannot just make two function calls side by side.

```JAVASCRIPT
function App() {
    return (
        <h1>This is the heading</h1>
        <p>Welcome to this space!</p>
    );
}
```

- **TIP: ALL OF THE JSX THAT WE WRITE, GETS TURNED INTO JS, AND HAS TO FOLLOW THE SAME RULES.**

- In react, there is a solution for this, it's called Fragments.

- So how do we solve for this?
- One option is to wrap both React elements in a div:

```JAVASCRIPT
return (
    <div>
      <h1>Welcome!</h1>
      <p>Don't forget to sign in</p>
    </div>
);
```

- This gets rendered to JS as, and we are no longer returning multiple expressions.

```JAVASCRIPT
return (
    React.createElement(
        'div',
        {},
        React.createElement('h1', {}, 'Welcome'),
        React.createElement('p', {}, 'Don\'t forget to sign guestbook'),
    )
)
```

- This is a fix, but not idea, and pollutes the DOM.
- It can also create accessibility and layout issues.

- **Better way with, Fragments**

- 🚀 A fragment is a special React component that doe snot product a DOM node. It looks like this:

```JAVASCRIPT
import React from 'react';

function App() {
  return (
    <React.Fragment>
      <h1>Welcome!</h1>
      <p>Don't forget to sign in.</p>
    </React.Fragment>
  );
}

export default App;
```

- If you inspect the output, you get the following;

```HTML
<body>
    <div id="root">
        <h1>Welcome!</h1>
        <p>Don't forget to sign in.</p>
    </div>
</body>
```

- Fragments allow us to respect the rules of JS without polluting the HTML.

#### Shorthand

- You can also create a react fragment with the following syntax:

```JAVASCRIPT
return (
  <>
    <h1>Welcome to my homepage!</h1>
    <p>Don't forget to sign the guestbook!</p>
  </>
);
```
