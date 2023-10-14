# Module 2 - Working with State

- [Course Outline Notes](course-notes.md)

## Component Instances

- An important concept in React, and rarely discussed. Whenever we render a component in Rect, **we create a component instance**.
- A lot of outdated info on this topic. React has changed a ton since then.

- Video Summary:
- Where does state live? Look at a minimal React app:

```JAVASCRIPT
    import React from 'react';
    import { createRoot } from 'react-dom/client';

    function RandomNumber() {
      const [num, setNum] = React.useState(0);

      return (
        <button onClick={() => setNum(Math.random())}>
          Current number: {num}
        </button>
      );
    }

    const root = createRoot(document.querySelector('#root'));
    root.render(
      <>
        <RandomNumber />
      </>
    );
```

- In this example; we have a react app with a little bit of state. When you click the button, we set some state in the `num`. But where does this value come from? Where does it live?
- In this example; when we call `root.render` and call the component `<RandomNumber />` **we mount this component**.

- Mounting a Component consist of two parts:
  - 1. The process of figuring out what the JSX is. Everything in the component and processing it, handlers, DOM elements and any logic. Take this description and hand it over to React, and React produces that component.
  - 2. The second part, is we create a component instance, the very first time we render a component, it creates an object. And that object knows all the information about this particular instance of the component. Each one, gets it's own instance and manged by React.

- So we mount the component, which creates this instance, and over time as we are changing the value, that value is stored on this instance.

- In another example, if you remove an instance from the DOM, conditionally render a component based on some handler. React will destroy that instance and any state that is held within that component.
- If state is stored in an instance of a component, and that component is removed by React, than you loose anything stored in state.
- The example below, state is stored in the footer. When you toggle the button to display the footer. When you toggle the footer off, React removes that instance, destroys it from the DOM, and any state stored within the footer.

```JAVASCRIPT
import React from 'react';

function App() {
  const [
    includeFooter,
    setIncludeFooter,
  ] = React.useState(false);

  return (
    <>
      <h1>Some Application</h1>
      <form className="footer-toggle-wrapper">
        <input
          type="checkbox"
          id="footer-toggle"
          checked={includeFooter}
          onChange={(event) => {
            setIncludeFooter(event.target.checked);
          }}
        />
        <label htmlFor="footer-toggle">
          Toggle Footer
        </label>
      </form>
      {includeFooter && <Footer />}
  
    </>
  );
}

function Footer() {
  const [
    backgroundColor,
    setBackgroundColor,
  ] = React.useState('#232538');

  return (
    <footer style={{ backgroundColor }}>
      <form>
        <label htmlFor="bg-picker">
          Tweak background:
        </label>
        <input
          type="color"
          id="bg-picker"
          value={backgroundColor}
          onChange={(event) => {
            setBackgroundColor(event.target.value);
          }}
        />
      </form>
      <p>
        © Some Application Inc., 1998-present. All
        Rights Reserved.
      </p>
    </footer>
  );
}

export default App;
```

- What happens if you add multiple footers?

```JAVASCRIPT
{includeFooter && <Footer />}
{includeFooter && <Footer />}
{includeFooter && <Footer />}
```

- Each footer is a separate instance. And each have their own state variable.
- Each instance is independent.
- The moment we stop rendering that instance, that element gets deleted by React.
- 🤔 Every time you render a component, you create an instance.

- 📣 CORE CONCEPT: State is tied to a particular component instance, so if you unmount the component, and then re-mount it, that state is gone forever.
- 📣 Or if you have the same component rendered multiple times, they all have their own version of state, because they are a separate instance.
