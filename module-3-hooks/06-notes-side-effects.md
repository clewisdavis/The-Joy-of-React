# The Joy of React - Module 3 - Course Notes

- [Course Outline Notes](../course-notes.md)

## React Hooks

## Side Effects

- As we build apps, we often need to synchronize with external systems. Things like.
  - Making network request
  - Managing timeouts / intervals
  - Reading / writing from localStorage
  - Listening for global events

- React calls all of these things 'side effects'.

### About "Best Practices"

- So, these side effects are hard, one of the hardest things about a modern React app.
- Should never feel shame for writing code that is not perfect and follow the best practices.
- Where ever you go, everyone is learning it as you go, and it's more about the journey and growth of learning.
- You will never be in a place in your career where you are writing/designing perfect code or designs. You are always learning. And if you look back at your code or designs from a couple years ago, and feel embarrassed, that is a good sign. That you are growing and not feeling stagnated.

### The useEffect hook

- So far, we have discussed the **CORE React Loop**
  - Provide React the JSX, a description of the UI you want.
  - Then React takes those descriptions and produces the real world DOM the user can see and use.
  - When the use does something, state change for example, React re-runs all the code and give React a new description of the DOM.

- But in web apps, we need to do things outside of this. For example, updating the title of the tab.
- This falls outside of Reacts normal space. Now we want to change something that exist outside of that container.
- React has a name for this, Side Effects, this is when you want to do something that is typically outside of Reacts typical job responsibilities. But still needs to be synchronized with the state we are holding in React.

- We want to synchronize this external element, with React state.
- To do this, we use `React.useEffect()`
- How it works, we pass it a function. And React will call this function for us at appropriate times.

- Example, update the title tag, to use a count state variable.

```JAVASCRIPT
// Counter.js

function Counter({ name, initialVal = 0 }) {

  // state management
  const [count, setCount] = React.useState(initialVal);

  React.useEffect(() => {
    document.title = `(${count}) - Counter 2.0`;
  })
  return (
    // DOM markup
  )
}
```

- What's going on here, with this function? We are telling React, to immediately run, after every render of this component.
- Whenever something updates, it will render the JSX, and then call the function inside the `useEffect()`.

- Why do you need `useEffect()`, couldn't you just put the function right in the body and have it render?

```JAVASCRIPT
// Counter.js

function Counter({ name, initialVal = 0 }) {

  // state management
  const [count, setCount] = React.useState(initialVal);

  //React.useEffect(() => {
  //  document.title = `(${count}) - Counter 2.0`;
  //})

  // Can't you just do this? Not considered good practice. 
  document.title = `(${count}) - Counter 2.0`;

  return (
    // DOM markup
  )
}
```

- Has to do with updates, and the re-render of the component. You shouldn't have to run the function everytime the component updates. In this example, only when `${count}` changes.

- The way it works, you specify a second argument as an array of dependencies.
- The first argument is the function and the second argument is the array of dependencies.

```JAVASCRIPT
// Counter.js

function Counter({ name, initialVal = 0 }) {

  // state management
  const [count, setCount] = React.useState(initialVal);

  React.useEffect(
    // first argument, function
    () => {
      document.title = `(${count}) - Counter 2.0`;
    },
    // second argument, list of dependencies
    [count]
  );
  return (
    // DOM markup
  )
}
```

- 🚀 Tells React, to only call the function, when the `count` variable changes.

- So instead of calling it on every single render, it only fires when something is updated in the second argument array, list of dependencies.
- But, the effect will always execute in the very first render. And the dependency array, will always tell React, how to re-render, when to fire the function.

### Exercises, useEffect

Get some practice with `useEffect` hook.

#### Logging a particular value

- Sign up form with several React state variables.
- Goal is to add a `console.log` that fires only when the value of "email" changes. We should see teh user's email logged in the console whenever they edit that field.

ACs:

- Whenever the user changes the value of the "email"state variable, the new value should be logged to the console.
- Nothing should be logged when the user changes their name, city or postal code.

Stretch Goal:

- Update the code so that `name` is also logged whenever it changes.

- Solution Code;
- TIP: ℹ️ You can have as many `useEffect()` hooks as you need so each only deals with a single concern.

```JAVASCRIPT
import React from 'react';

function SignupForm() {
  const [name, setName] = React.useState('');
  const [email, setEmail] = React.useState('');
  const [city, setCity] = React.useState('');
  const [postalCode, setPostalCode] = React.useState(
    ''
  );

  React.useEffect(
    // function
    () => {
      console.log({name});
    },
    // dependencies
    [name]
  );

    React.useEffect(
    // function
    () => {
      console.log({email});
    },
    // dependencies
    [email]
  );

  return (
    <form>
      <Field
        id="name"
        label="Preferred Name"
        value={name}
        onChange={(event) => {
          setName(event.target.value);
        }}
      />
      <Field
        id="email"
        type="email"
        label="Email Address"
        value={email}
        onChange={(event) => {
          setEmail(event.target.value);
        }}
      />
      <div className="row">
        <Field
          id="city"
          label="City"
          grow={2}
          value={city}
          onChange={(event) => {
            setCity(event.target.value);
          }}
        />
        <Field
          id="postal-code"
          label="Postal Code"
          grow={1}
          value={postalCode}
          onChange={(event) => {
            setPostalCode(event.target.value);
          }}
        />
      </div>
      <button>Sign up</button>
    </form>
  );
}

function Field({
  id,
  label,
  type = 'text',
  grow,
  value,
  onChange,
}) {
  return (
    <div
      className="field"
      style={{ '--grow': grow }}
    >
      <label htmlFor={id}>{label}</label>
      <input
        type={type}
        id={id}
        value={value}
        onChange={onChange}
      />
    </div>
  );
}

export default SignupForm;
```

### Exercise, Locally Persisted State

Imagine you are building an app with a "Dark Mode" toggle. It would be annoying if users had to keep toggling their preferred mode every time they load the application.

- Update the code ts that the user's preference is saved in localStorage, and restored when the page reloads.
- The current value of `isDarkMode` should be "remembered", and usd whn the page is refreshed.

- In plain JS, we can do this with the following code:

```JAVASCRIPT
// Save the value;
window.localStorage.setItem('is-dark-mode', true);

// Retrieve the value;
window.localStorage.getItem('is-dark-mode');
```

- AC's
  - The value of `isDarkMode` should be saved in localStorage whenever it changes, using the `useEffect` hook.
  - The initial value of the `isDarkMode` state variable should be retrieved from localStorage (or set to `false` if no value has been saved).
  - You can use the string `"is-dark-mode"` for the key.
  - Note: Items saved to localStorage are always saved as a string. You will need to convert the stored value back to boolean.  You can do this with the `JSON.parse()`.

- ℹ️ Check out the [Local Storage troubleshooting guide](https://courses.joshwcomeau.com/support/local-storage-troubleshooting) from Josh.

- Set into storage:

```JAVASCRIPT
function App() {
  const [isDarkMode, setIsDarkMode] = React.useState(false);

  React.useEffect(
    () => {
      // save to local storage
      window.localStorage.setItem('is-dark-mode', isDarkMode);
    },
    [isDarkMode]
  );

  return (
    // JSX here
  )
```

- Get it from storage:
- Note: `localStorage` is only capable of storing string, you have to convert it to the type you want.

```JAVASCRIPT
function App() {
  
  // get from localStorage
  const storedValue = window.localStorage.getItem('is-dark-mode');
  // convert the localStorage string to a boolean
  const parsedValue = JSON.parse(storedValue);

  // then use the parsedValue as your initial state
  const [isDarkMode, setIsDarkMode] = React.useState(parsedValue);

  React.useEffect(
    () => {
      // save to local storage
      window.localStorage.setItem('is-dark-mode', isDarkMode);
    },
    [isDarkMode]
  );

  return (
    // JSX here
  )
```

- But, need to address `null` value when nothing is specified on initial load.
- Update the `parsedValue` with an OR conditional `||` so it will default to `false`

```JAVASCRIPT
  const parsedValue = JSON.parse(storedValue) || false;
```

- One last thing, we don't want to run `window.localStorage` on every single render, it is expensive to interact with local storage.
- You can use the 'alternative form factor', whatever that means, in the `useState`. Use a callback function inside of state.
- On initial load, it will use the callback function to figure out what the value should be, but on every render after that, it won't run.

```JAVASCRIPT
function App() {

  // then use the parsedValue as your initial state
  const [isDarkMode, setIsDarkMode] = React.useState(
    // Callback function to determine the initial value on the initial render only.
    () => {
      // get from localStorage
      const storedValue = window.localStorage.getItem('is-dark-mode');
      // return the value
      return JSON.parse(storedValue) || false;
    }
  );

  React.useEffect(
    () => {
      console.log(isDarkMode);
      // save to local storage
      window.localStorage.setItem('is-dark-mode', isDarkMode);
    },
    [isDarkMode]
  );

  return (
    // JSX here
  )
```

### Effect Lint Rules

- A common linting error, when using `useEffect`, is `React Hook React.useEffect has a missing dependency: 'count'. Either include it or remove the dependency array.`
- Not a good practice to silence the warning, with `// eslint-disable-next-line`, you are not solving the problem, you are just silencing the problem.

- The problem is the state variables are getting out of sync, every time you re-render the component. The snapshots are using different versions of the `count` variable stored in memory.
- To solve this, make sure to always include the state variable dependency in the `useEffect` dependency array.

- Simple Counter example:

```JAVASCRIPT
import React from 'react';

function App() {
  const [count, setCount] = React.useState(0);
 
  // Lint Warning: React Hook React.useEffect has a missing dependency: 'count'. Either include it or remove the dependency array.
  React.useEffect(() => {
    console.log(count);
  }, []);

  return (
    <>
      <p>The count is: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </>
  );
}

export default App;
```

- To solve the warning, include the `count` state variable as dependency.

```JAVASCRIPT
import React from 'react';

function App() {
  const [count, setCount] = React.useState(0);
 
  // Include the count state variable.
  React.useEffect(() => {
    console.log(count);
  }, [count]);

  return (
    <>
      <p>The count is: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </>
  );
}

export default App;
```

### Running on Mount

- Imagine you have a form, and when the page loads, you want focus on the input.
- How, or when do you call the `focus()` method on the input?
- The issue is timing, when the component mounts, and when things render.

```JAVASCRIPT
import React from 'react';

function App() {
  const [
    searchTerm,
    setSearchTerm,
  ] = React.useState('');
  
  // create a reference, { current: undefined}
  const inputRef = React.useRef();
  
  // 🛑 wrong, is undefined because it runs before the JSX below.
  inputRef.current.focus();

  return (
    <>
      <header>
        <img
          className="logo"
          alt="Foobar"
          src="https://sandpack-bundler.vercel.app/img/foogle.svg"
        />
      </header>
      <main>
        <form>
          <input
            ref={inputRef}
            value={searchTerm}
            onChange={(event) => {
              setSearchTerm(event.target.value);
            }}
          />
          <button>Search</button>
        </form>
      </main>
    </>
  );
}

export default App;
```

- The issue is timing, React needs to render the JSX, to create the input and all the DOM nodes.
- What you want to do, is wait until React has run the JSX, and then run the `focus()` method.
- Common in React, if you want to make a network request, or establish a DOM node.

- The established way, is the `useEffect()` Hook.

- But, with this code, effect run after every single render. Which is not exaclty what you want.

```JAVASCRIPT
React.useEffect(() => {
  inputRef.current.focus();
});
```

- To solve this, add a dependency array to the effect. And leave it empty.
- 🤔 Telling React, you only want to run when one of these dependencies change, but you haven't specified any dependencies. So nothing changes after the mount.
- It will always run after that first mount, but will never run again, no matter what state changes happen.

```JAVASCRIPT
React.useEffect(() => {
  inputRef.current.focus();
}, []);
```

- Full example, search input focus() with useEffect Hook:

```JAVASCRIPT
import React from 'react';

function App() {
  const [
    searchTerm,
    setSearchTerm,
  ] = React.useState('');

  const inputRef = React.useRef();

  React.useEffect(() => {
    // Uncomment me!
    // inputRef.current.focus()
  }, []);

  return (
    <>
      <header>
        <img
          className="logo"
          alt="Foobar"
          src="https://sandpack-bundler.vercel.app/img/foogle.svg"
        />
      </header>
      <main>
        <form>
          <input
            ref={inputRef}
            value={searchTerm}
            onChange={(event) => {
              setSearchTerm(event.target.value);
            }}
          />
          <button>Search</button>
        </form>
      </main>
    </>
  );
}

export default App;
```

### Subscriptions

- Let's suppose you want to track the user's cursor position. Whenever they move their mouse, we will update some state. We can add `onMouseMove` event handlers to specific DOM nodes like this:

```JAVASCRIPT
<div
  onMouseMove={event => {
    setMousePosition({
      x: event.clientX,
      y: event.clientY,
    });
  }}
>
```

- This will only work while the user is hovering over this particular `<div>` though. What if we want to track their cursor position no matter where the mouse is within the viewport?

- Try the solution, to work on the solution.
- HINT: To listen to global events, you can use `window.addEventLister`. You want to listen for `mousemove` events. You can get the cursor position using `event.clientX` and `event.clientY`.

- Solution:
- Create a `useEffect()` hook to manage the global mouse position.
- And use the set state function and pass it an object, in the same format as the `useState` object.

```JAVASCRIPT
import React from 'react';

function MouseCoords() {
  const [
    mousePosition,
    setMousePosition
  ] = React.useState({ x: 0, y: 0 });

  React.useEffect(() => {
    function handleMouseMove(event) {
      // state setter, object in same format
      setMousePosition({
        x: event.clientX,
        y: event.clientY,
      });
    }
  })
  
  return (
    <div className="wrapper">
      <p>
        {mousePosition.x} / {mousePosition.y}
      </p>
    </div>
  );
}

export default MouseCoords;
```

- Set a global event listener, pass in the `mousemove` event, and pass in the `handleMouseMove` callback.

```JAVASCRIPT
import React from 'react';

function MouseCoords() {
  const [
    mousePosition,
    setMousePosition
  ] = React.useState({ x: 0, y: 0 });

  React.useEffect(() => {
    function handleMouseMove(event) {
      // state setter, object in same format
      setMousePosition({
        x: event.clientX,
        y: event.clientY,
      });
    }
      // set the global event listener
      window.addEventListener('mousemove', handleMouseMove);
  })

  return (
    <div className="wrapper">
      <p>
        {mousePosition.x} / {mousePosition.y}
      </p>
    </div>
  );
}

export default MouseCoords;
```

- Now, when you move the mouse, you see the numbers update on the DOM.
- But, a problem with this.
- We are adding an event listener, but we are never removing the event listener.
- The problem is, we are adding an event listener, every time we move the mouse. This piles up over time.

- When you call `window.addEventListener()` it is purely a JS thing, happens outside of React.
- Every time, you move your cursor, it re-runs the component, draws the JSX and runs the `useEffect()` hook again, which includes the `window.addEventListener`.
- This creates a new event listener every single time.

- Think of this method, `window.addEventListener()` as a subscription, by starting it, you are starting a long running process.
- 📣 You only want to do that once, when the component mounts 📣
- And the long running process will handle all the `mousemove` events

- To fix this, add an empty dependency array `[]` to the `useEffect()` hook. Which means, the event listener will only run the first time it renders.

```JAVASCRIPT
  React.useEffect(() => {
    function handleMouseMove(event) {
      // state setter, object in same format
      setMousePosition({
        x: event.clientX,
        y: event.clientY,
      });
    }
      // set the global event listener
      window.addEventListener('mousemove', handleMouseMove);
  }, []);
```

- 📣 This is the way it works with other types of subscription, starting an interval, or starting a web socket connection.
- You want to start the process once, and then the process takes care of itself. Updating the state as the events happen.

- Diagram, with an empty dependency array.
- Initial render, that starts the long running process. And for every re-render, it only runs the event.

![Alt text](images/image.png)

- Diagram, With no dependency array
- Every time we trigger an event, it creates a re-render, but you also start another `Effect`, which they start to pile on top of one another.

![Alt text](images/image-1.png)

- 🤔 This is very common, where you have some sort of subscription or registration that needs to happen immediately after mount. But them we need to make sure it doesn't happen over and over again. **Because the subscription is long running.**

### Exercises, Side Effects

Exercises for side effects

#### Window dimensions

Let's use teh effect hook to track teh window's dimensions over time.

- ACs
  - As the window is resized, teh number shown in the "Result" tab should update, accurately showing the width and height of the iframe.
  - Only a single event listener should be registered.

NOTE:  You can test this by dragging the division between teh two tabs. If you are not using a pointer device, you can focus the divider and use the left/right arrow keys.

- My Solution:

```JAVASCRIPT
import React from 'react';

function WindowSize() {
  const [
    windowDimensions,
    setWindowDimensions,
  ] = React.useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  // Add use effect hook
  React.useEffect(() => {
    // Create the callback function and call our set state handler
    function handleResize() {
      setWindowDimensions({
        width: window.innerWidth,
        height: window.innerHeight,
      })
    }
    // Set the global event listener on resize and call our function
    window.addEventListener('resize', handleResize);
    // Add the empty dependency array below, so the listener will only run once
  }, []);
  
  return (
    <div className="wrapper">
      <p>
        {windowDimensions.width} / {windowDimensions.height}
      </p>
    </div>
  );
}

export default WindowSize;
```

#### Exercises, Toasty

- Create a toasty effect, tied to a scroll position. When the user scrolls to a specific point in the page, a message will slide out from the edge of the screen.
- Lots going on, video for context.

- ACs
  - As the use scrolls near the bottom of the page, a ghost character should slide in.
  - This can be accomplished by observing the `styles.wrapper` element, and setting the `isShown` state variable to true/false based on whether it's within the viewport or not.
  - You shouldn't use `document.querySelector` You can capture a reference to the wrapper element with the `React.useRef` hook.

- ℹ️ Note: This uses the `IntersectionObserver()` API. How it works, you create an intersection observer instance, and then tell it to observe a particular DOM node.
- Similar to an event listener, that is starts a long running process. `observer.observe(wrapperElement)`
- Pure JS version:

```JAVASCRIPT
// Here's how we'd solve this problem using vanilla JS:
function pureJsVersion() {
  const wrapperElement = document.querySelector('.toasty-wrapper');
  
  const observer = new IntersectionObserver((entries) => {
    const [entry] = entries;

    if (entry.isIntersecting) {
      // Show character
    } else {
      // Hide character
    }
  });

  observer.observe(wrapperElement);
}
```

- TODO:
  - Get the DOM element to reference, `useRef`
  - Create a `useEffect` hook, with callback function for observer

- Solution:

```JAVASCRIPT
// Toasty.js
import React from 'react';

import styles from './Toasty.module.css';

// Here's how we'd solve this problem using vanilla JS:
// function pureJsVersion() {
//   const wrapperElement = document.querySelector('.toasty-wrapper');
  
//   const observer = new IntersectionObserver((entries) => {
//     const [entry] = entries;

//     if (entry.isIntersecting) {
//       // Show character
//     } else {
//       // Hide character
//     }
//   });

//   observer.observe(wrapperElement);
// }

function Toasty() {
  // Your goal is to update the `isShown` state variable,
  // based on the user's scroll position, using
  // IntersectionObserver.
  const [isShown, setIsShown] = React.useState(false);

  // Get the reference
  const wrapperRef = React.useRef();
  console.log(wrapperRef); // {current: undefined

  // Create a useEffect hook to call the Intersection Observer
  React.useEffect(() => {

    const observer = new IntersectionObserver((entries) => {
      const [entry] = entries;
      console.log({entry});
      console.log(entry.isIntersecting)
      
      // if (entry.isIntersecting) {
      //   // Show character
      //   setIsShown(true)
      // } else {
      //   // Hide character
      //   setIsShown(false)
      // }

      // set the state variable directly
      setIsShown(entry.isIntersecting);
    }); 

    // Listen for the scroll position using the useRef hook
    observer.observe(wrapperRef.current);
  }, []);
   
  // This CSS value will control whether the ghost is
  // visible or not.
  const translateX = isShown
    ? '0%'
    : '100%';
  
  return (
    <div ref={wrapperRef} className={styles.wrapper}>
      <div
        className={styles.character}
        style={{
          transform: `translateX(${translateX})`,
        }}
      >
        👻
      </div>
    </div>
  );
}

export default Toasty;
```

- Why are we not getting any dependency warnings, for using `wrapperRef`, or `setIsShown`? These things are declared outside of the effect.
- A state variable can grow stale over time, but the set state function `setIsShown`, is the same function every single time. We get this from React every single time.
- Same thing with `const wrapperRef = React.useRef()`, is it always the same object over time, the object is the same each time, but the values inside the object update over time. There are not multiple copies of the object for every single snapshot. Share the same object across renders.

- So that means, the values cannot become stale, and no issues with using them in an effect.

- Bonus: [See video](https://courses.joshwcomeau.com/joy-of-react/03-hooks/05.05-on-mount-exercises) for how the CSS works on this effect.
- ☝️ Good example of conditionally trigger some CSS from an event.

#### Cleanup

- All the solutions so far, with the subscriptions, are incomplete.
- When React mounts a component, all the subscriptions / logic or long running processes with that component are started.
- When you unmount a component, shouldn't the opposite happen? It would stop or remove any long running process?
- No, it doesn't, even when you unmount a component instance, the event lister is still running in the background.
- And ever time you mount and unmount a component with a subscription (listener) you are stacking them on top of one another.
- 📣 They are never being cleaned up. 📣

- We say that when we un-mount the component, we delete the instance. We don't actually delete the instance, we just remove references to it. It's just out there floating in memory.

- The JS garbage collector 🤔 cannot clean it up, if there are still references to it. If a event listener or subscription is still running.

- If the subscription is still running, that means the component instance can not be cleaned up, deleted, destroyed.
- So you end up with new ones, being stacked up in memory over time.

- The reason it doesn't get cleaned up, in the `useEffect()` hook, the subscription, or event listener is not part of React. It is part of vanilla JS.

```JAVASCRIPT
  React.useEffect(() => {
    // Effect logic:
    function handleMouseMove(event) {
      console.log('move');
      setMousePosition({
        x: event.clientX,
        y: event.clientY,
      });
    }

    // Part of vanilla JS, will not get cleaned up in React:
    window.addEventListener('mousemove', handleMouseMove);

    // Add a Cleanup function to remove:
    return () => {
      window.removeEventListener('mousemove', handleMouseMove);
    };
  }, []);
```

- And actually, when we call a `useEffect()`, React doesn't have any idea what is inside that effect. It just calls it.
- It just says, hey React, here is a function that I would like you to run, when this component mounts. And here are the dependencies, for when this effect should re-run.
- 🤔 We are giving React a chunk of code, and when it should run.
- React has no idea, you are starting an event listener, when you mount the component, or when it runs the `useEffect`

- You have two independent systems, React and Vanilla JS, that are going about, doing their own thing.
- 🚀 It is up to us, to make sure these things are synchronized. When the component unmounts, **we remove the ongoing events**. 🚀

- In vanilla JS, you can use the `removeEventListener()`, you want to run this, right before the component unmounts.
- The `useEffect()` hooks comes with a tool for this. The way it works, is you give it a `return()` function and put anything in it you want to clean up.

- React now has two different things it can do at different times.
- When the component mounts, it will run the effect logic

```JAVASCRIPT
    // Effect logic:
    function handleMouseMove(event) {
      console.log('move');
      setMousePosition({
        x: event.clientX,
        y: event.clientY,
      });
    }
```

- Then it will return a function, and React is going to hand on to this function.
- 🤔 React will evoke this function at the right time, right before the component un-mounts.
- Which will remove the ongoing event listener.

```JAVASCRIPT
    // Add a Cleanup function to remove:
    return () => {
      window.removeEventListener('mousemove', handleMouseMove);
    };
```

- 🚀 When you are dealing with any ongoing subscription, this is the format. You set it up, and then return a function to manage the clean up. 🚀

- Full example of Mouse tracker

```JAVASCRIPT
import React from 'react';

function MouseTracker() {
  const [mousePosition, setMousePosition] = React.useState({
    x: 0,
    y: 0,
  });

  React.useEffect(() => {
    // Effect logic:
    function handleMouseMove(event) {
      console.log('move');
      setMousePosition({
        x: event.clientX,
        y: event.clientY,
      });
    }

    window.addEventListener('mousemove', handleMouseMove);

    // Cleanup function:
    return () => {
      window.removeEventListener('mousemove', handleMouseMove);
    };
  }, []);

  return (
    <p>
      {mousePosition.x} / {mousePosition.y}
    </p>
  );
}

export default MouseTracker;
```

- 😬 Functions that return functions 😬
- This is a confusing idea. A function that all it does, it return another function.

```JAVASCRIPT
function doSomething() {
  const hi = 5;

  return function() {
    return hi * 2;
  };
}
```

- A similar thing is happening with the `useEffect()` cleanup API. Our main effect function is returning a function:
- The good news, this is just an implementation detail.
- The most important thing to remember, We give React two functions, an effect function and a cleanup function. And React calls these functions for us at the appropriate time.

#### Cleanup with dependencies

- So far, in the examples, we have learned how to start a long running process the moment the component mounts, and we have learned how to clean it up.
- We don't include a dependency in the dependency array and that tells React, to not re-run this code.

```JAVASCRIPT
  React.useEffect(() => {
    function handleMouseMove(event) {
      setMousePosition({
        x: event.clientX,
        y: event.clientY,
      });
    }

    window.addEventListener('mousemove', handleMouseMove);

    return () => {
      window.removeEventListener('mousemove', handleMouseMove);
    };
  }, []);
```

- What happens when we do have dependencies, and we want the code to re-run sometimes?
- Want to use a state variable `isEnabled` to start and stop the event listener.

```JAVASCRIPT
const [isEnabled, setIsEnabled] = React.useState(true);
```

- Cannot wrap in a conditional, even though this may seem like the intuitive way.
- ✋ React will not allow you to call `useEffect()` hook conditionally.

```JAVASCRIPT
  const [isEnabled, setIsEnabled] = React.useState(true);

  // 🛑 Will not work, wrapping conditionally
  if (isEnabled) {
    React.useEffect(() => {
      function handleMouseMove(event) {
        setMousePosition({
          x: event.clientX,
          y: event.clientY,
        });
      }

      window.addEventListener('mousemove', handleMouseMove);

      return () => {
        window.removeEventListener('mousemove', handleMouseMove);
      };
    }, []);
  }

```

- But the idea, is a good one. 🤔 You can move the condition to be just inside the `useEffect()`.
- So the entire body of the effect, including the clean up function, are wrapped in a condition.

```JAVASCRIPT
  const [isEnabled, setIsEnabled] = React.useState(true);

  // 👍 Will work, condition inside the effect, but you need to include the dependency
  React.useEffect(() => {
    if (isEnabled) {
      React.useEffect(() => {
        function handleMouseMove(event) {
          setMousePosition({
            x: event.clientX,
            y: event.clientY,
          });
        }

        window.addEventListener('mousemove', handleMouseMove);

        return () => {
          window.removeEventListener('mousemove', handleMouseMove);
        };
      }, [isEnabled]);    
    }
```

- Now, our mouse tracker works, and I can start and stop the tracking process.
- But how is this working?

1. Refresh the page, the component mounts for the first time. And we run the `useEffect`
2. The state variable is set to true, `isEnabled`. so we run the `handleMouseMove` event to start the listener. And also hand React the cleanup function `return` for later.
3. All the tracking happens as the user move mouse around. No dependencies updated.
4. Then the user clicks the button, and it updates the dependency, `isEnabled` to be `false` which triggers a re-run of the effect.
5. But before it can do that, it has to invoke the clean up function. React always wants to clean up, what was done previously, before it starts a new effect. So it removes that event listener, `window.removeEventListener()`, in the return function.
6. Then React re-runs the effect, except now, `isEnabled` is false, and it skips over all the code inside the body of the effect. Like commenting out everything inside the `if(isEnabled) {}` condition.
7. Now, we have no mouse tracking, until the user clicks the button again, and runs the effect and everything starts over again.

- Full code example, mouse tracker, with cleanup dependency.

```JAVASCRIPT
import React from 'react';

function MouseTracker() {
  const [mousePosition, setMousePosition] = React.useState({
    x: 0,
    y: 0,
  });
  const [isEnabled, setIsEnabled] = React.useState(true);

  React.useEffect(() => {
    if (isEnabled) {
      function handleMouseMove(event) {
        setMousePosition({
          x: event.clientX,
          y: event.clientY,
        });
      }

      window.addEventListener('mousemove', handleMouseMove);

      return () => {
        window.removeEventListener('mousemove', handleMouseMove);
      };
    }
  }, [isEnabled]);

  function toggleMouseTracking() {
    setIsEnabled(!isEnabled);
  }

  return (
    <>
      <button onClick={toggleMouseTracking}>
        Mouse Tracking: {isEnabled ? 'On' : 'Off'}
      </button>
      <p>
        {mousePosition.x} / {mousePosition.y}
      </p>
    </>
  );
}

export default MouseTracker;
```

- Graph examples:

- The order of operations: Shows what effects are scheduled based on what render it is.
- Effects always run after the first render.
- No cleanup, haven't done anything yet. **If a cleanup function is provided, it will hang on to it.**

![Alt text](images/image-2.png)

- A view of individual snapshots and cleanup:
- You want to stop running the effect from the first render, before you start something else.
- On each subsequent re-render, the cleanup function is invoked before running the effect again, and when the component is un-mounted.

![Alt text](images/image-3.png)

- Cleanup functions aren't always provided:
- You don't always have to provide a cleanup, if there is nothing to clean up, no subscription or event listener running.
- Like the conditional example above, if `isEnabled` is false, none of the code inside the effect runs. So, there is nothing to clean up.

![Alt text](images/image-4.png)

- The big idea 🤔, you want to provide a cleanup function, if the effect starts any sort of ongoing process. For events, intersection observer, timeout, or an interval. 📣 You want to make sure you provide a way to stop the subscription, so they don't stake up.

#### Exercises, Cleanup Subscriptions

- Practice some cleanup subscriptions with useEffect Hook.

##### Fixing Previous exercises

**Window dimensions:**

- Update the solution to the "Window dimensions" exercise so that the event listener is cleaned up.

AC's

- The "resize" event listener should be removed inside a cleanup function.

Solution:

```JAVASCRIPT
import React from 'react';

function WindowSize() {
  const [
    windowDimensions,
    setWindowDimensions,
  ] = React.useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  React.useEffect(() => {
    function handleResize() {
      console.log('resize');
      
      setWindowDimensions({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    }

    window.addEventListener('resize', handleResize);

    // Add the cleanup function to remove the event listener
    return() => {
      window.removeEventListener('resize', handleResize);
    }
  }, []);

  return (
    <div className="wrapper">
      <p>
        {windowDimensions.width} / {windowDimensions.height}
      </p>
    </div>
  );
}

export default WindowSize;
```

**Toasty!**

- In the Toasty exercise, we implemented a animation using the IntersectionObserver, but we never cancel the process.
- Here is the code to stop the IntersectionObserver. `observer.disconnect()`

AC's

- The IntersectionObserver should be disconnected within the effect's cleanup function.

Solution:

```JAVASCRIPT
/*

To stop an IntersectionObserver, you can run:
  observer.disconnect();

*/
import React from 'react';

import styles from './Toasty.module.css';

function Toasty() {
  const [isShown, setIsShown] = React.useState(false);

  const wrapperRef = React.useRef();

  React.useEffect(() => {
    const observer = new IntersectionObserver((entries) => {
      const [entry] = entries;

      setIsShown(entry.isIntersecting);
    });

    observer.observe(wrapperRef.current);

    // Cleanup function to remove the observer, no good way to test this though
    return() => {
      observer.disconnect();
    }
  }, []);

  const translateX = isShown ? '0%' : '100%';

  return (
    <div ref={wrapperRef} className={styles.wrapper}>
      <div
        className={styles.character}
        style={{
          transform: `translateX(${translateX})`,
        }}
      >
        👻
      </div>
    </div>
  );
}

export default Toasty;
```

##### Digital Clock

Build a digital clock in React! We have a clock starter code but the value doesn't update. Update the code so that the clock shows the correct time:

AC's

- The clock should update once per second, to show the current time.
- There should be no memory leaks, no ongoing processes that outlive the `Clock` instance.

Hint: Brush up on your JS, "[Intervals and Timeouts](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/13-intervals-and-timeouts)"

```JAVASCRIPT
// Clock.js, Starter Code
import React from 'react';
import format from 'date-fns/format';

function Clock() {
  const [time, setTime] = React.useState(new Date());

  return (
    <p className="clock">
      {format(time, 'hh:mm:ss a')}
    </p>
  );
}

export default Clock;
```

```JAVASCRIPT
// App.js, to toggle the clock on and off, mount and unmount the Clock.js component
import React from 'react';
import format from 'date-fns/format';

function Clock() {
  const [time, setTime] = React.useState(new Date());

  React.useEffect(() => {
    // effect function
    const clockInterval = window.setInterval(() => {
      console.log('tick');
      setTime(new Date());
    }, 1000);

    // cleanup
    return() => {
      window.clearInterval(clockInterval);
      console.log('cleanup');
    }
  }, [time]);
  return (
    <p className="clock">
      {format(time, 'hh:mm:ss a')}
    </p>
  );
}

export default Clock;
```

- Solution:
- Use the `setInterval` method to call the `setTime` state setter.

```JAVASCRIPT
import React from 'react';
import format from 'date-fns/format';

function Clock() {
  const [time, setTime] = React.useState(new Date());

  React.useEffect(() => {
    // effect function
    const clockInterval = window.setInterval(() => {
      console.log('tick');
      setTime(new Date());
    }, 1000);

    // cleanup
    return() => {
      window.clearInterval(clockInterval);
      console.log('cleanup');
    }
  }, []);
  return (
    <p className="clock">
      {format(time, 'hh:mm:ss a')}
    </p>
  );
}

export default Clock;
```

- 🤔 Think of the interval inside of an effect, like an event listener, it is just a long running process. Where we are starting a process in the `setInterval` and then ending it with the `clearInterval`. And there is some sort of even that happens in between. Where that's mouse move events or intersections, here it's the interval firing once per second.

#### Useless Machine

Useless machines, small boxes are built with a single switch on them, when you flip the switch, the box flips back. Let's build something similar.

We will create a checkbox that automatically ticks itself, whenever it's unticked.

AC's

- When the user unticks the checkbox, it should become re-ticked automatically after half a second. This can be done with a `setTimeout`.
- If the user toggles it off and on really quickly, the timeout should be canceled, since the machine has been left in an "on" state.
- If the user clicks it a bunch of times in rapid succession, the machine should wait a full 500ms from the final click before ticking itself back on.

Notes:

- When you have a situation where you want to change state or something after a specific amount of time, but you don't want it to keep doing it over and over again. You don't want to use `setInterval`, use `setTimeout` instead.
- Start with adding your `useEffect` dependency, only want to start this `timeout` when the `isOn` state variable changes.

```JAVASCRIPT
// state hook
const [isOn, setIsOn] = React.useState(true);

React.useEffect(() => {
  window.setTimeout(() => {
    // run this code every second, call the state setter and set to true
    setIsOn(true);
  }, 1000)
}, [isOn])
}
```

- How do you handle, if user clicks the checkbox on and off really quickly? You can add a condition at the beginning of the `useEffect`, to check the value of the state variable `isOn === true`. If the value is true, the `return` statement, will "bail early" and not run any of the code under it.
- This mean, we will only schedule a timeout, if the user toggles it off.
- ℹ️ See more info on [Early Returns](https://courses.joshwcomeau.com/joy-of-react/03-hooks/05.07-cleanup-exercises).

```JAVASCRIPT
  React.useEffect(() => {
    // condition to check value of isOn, to bail out of the effect early and not run the timeout code. 
    if (isOn === true) {
      return; // early return, this will bail out and prevent any code after the return to not run.
    }
    // function will only run, if isOn is false
    const timeoutId = window.setTimeout(
      () => {
        console.log('clicked')
        setIsOn(true);
      },
      500 // half second
    )

    // cleanup
    return() => {
      window.clearTimeout(timeoutId); 
      console.log('cleanup');
    }
  }, [isOn]);
```

- Full Code Example;

```JAVASCRIPT
import React from 'react';

import VisuallyHidden from './VisuallyHidden';

function UselessMachine() {
  const id = React.useId();
  const [isOn, setIsOn] = React.useState(true);

  React.useEffect(() => {
    // Ignore renders that happen because the
    // checkbox is flipped on. We only want to
    // respond when it's flipped *off*.
    if (isOn) {
      return;
    }

    // Flip the checkbox back on in 500ms...
    const timeoutId = window.setTimeout(() => {
      setIsOn(true);
    }, 500);

    return () => {
      // ...Unless the 'isOn' property changes
      // before that time has elapsed, OR the
      // component happens to unmount:
      window.clearTimeout(timeoutId);
    };
  }, [isOn]);

  return (
    <>
      <input
        id={id}
        type="checkbox"
        checked={isOn}
        onChange={(event) => {
          setIsOn(event.target.checked);
        }}
      />
      <VisuallyHidden>
        <label htmlFor={id}>Toggle checkbox</label>
      </VisuallyHidden>
    </>
  );
}

export default UselessMachine;
```

### Stale Values

- The media player example, from a previous exercise. One thing all media players have in common, the space bar key will play or pause the currently selected song.
- Update the media player  to include the space bar shortcut.

- ℹ️ Hint: You will want to register the `keydown` event listener. If they pressed the space bar, `event.code` will be equal to the string "Space".

- 🤔 To solve this problem, use multiple `useEffect` hooks, to keep the internal audio sync, `play()`, `pause()`, in sync with the `isPlaying` state variable.
- The alternative would be to just create a function to handle the playing logic.
- Better approach to handle in a effect, so you have a single source of truth for the state variable `isPlaying`

- **Solving with `isPlaying` as a dependency**

![Alt text](images/image-5.png)

```JAVASCRIPT
// MediaPlayer.js Component
import React from 'react';
import { Play, Pause } from 'react-feather';

import VisuallyHidden from './VisuallyHidden';

function MediaPlayer({ src }) {
  const [isPlaying, setIsPlaying] = React.useState(false);

  const audioRef = React.useRef();

  React.useEffect(() => {
    // function
    function handleKeyDown(event) {
      if (event.code === 'Space') {
        // Todo: play or pause
        // use the state setter function to update the state variable
        setIsPlaying(!isPlaying);
      }
    }

    // Set the event listener
    window.addEventListener('keydown', handleKeyDown);

    // Cleanup
    return () => {
      window.removeEventListener('keydown', handleKeyDown);
    }
  }, [isPlaying]);

  // Add a second useEffect hook to sync the audio and the state variable isPlaying
  // The one place, to manage the connection between the audio and the state variable
  React.useEffect(() => {
    if (isPlaying) {
      audioRef.current.play();
    } else {
      audioRef.current.pause();
    } 
  }, [isPlaying])

  return (
    <div className="wrapper">
      <div className="media-player">
        <img alt="" src="https://sandpack-bundler.vercel.app/img/take-it-easy.png" />
        <div className="summary">
          <h2>Take It Easy</h2>
          <p>Bvrnout ft. Mia Vaile</p>
        </div>
        <button
          onClick={() => {
            if (isPlaying) {
              audioRef.current.pause();
            } else {
              audioRef.current.play();
            }

            setIsPlaying(!isPlaying);
          }}
        >
          {isPlaying ? <Pause /> : <Play />}
          <VisuallyHidden>Toggle playing</VisuallyHidden>
        </button>

        <audio
          ref={audioRef}
          src={src}
          onEnded={() => {
            setIsPlaying(false);
          }}
        />
      </div>
    </div>
  );
}

export default MediaPlayer;
```

- **Solving with callback function:**

- Alternative approach to the dependency and adding and removing the event listener every time.
- In the effect, `handleKeyDown` function we call the state setter function `setIsPlaying` to switch the value of the state so the audio will play.

```JAVASCRIPT
  React.useEffect(() => {
    function handleKeyDown(event) {
      if (event.code === 'Space') {
        // Todo: play or pause
        // use the state setter function to update the state variable
        setIsPlaying(!isPlaying);
      }
    }

    // rest of useEffect code
  }
```

- But you can also pass a function, and return a value, it's equivalent to calling the setter function with that value.

```JAVASCRIPT
// pass a function with the value, this will return and set it to 5
setIsPlaying(() => {
  return 5
})

// equivalent to this
setIsPlaying(5)
```

- React will call this function for us and whatever you return, becomes the new state variable.
- Why is this useful, because we can set a parameter, and the parameter is the value of whatever the state value is.

```JAVASCRIPT
SetIsPlaying((currentIsPlaying) => {
  return
})
```

- Instead of trying to read the state variable from the snapshot, **which could be stale**, you can instead pass React a function
- 🚀 And ask React, you tell me what the current value is, and then I will do something with that value.
- 📣 I will do something with that value, I will manipulate it in some way, to calculate the next state.

- In this case, to flip the value of the state variable to start and stop the audio player

```JAVASCRIPT
SetIsPlaying((currentIsPlaying) => {
  return !currentIsPlaying;
})
```

- **Solving using a callback function:**

![Alt text](images/image-6.png)

```JAVASCRIPT
import React from 'react';
import { Play, Pause } from 'react-feather';

import VisuallyHidden from './VisuallyHidden';

function MediaPlayer({ src }) {
  const [isPlaying, setIsPlaying] = React.useState(false);

  const audioRef = React.useRef();

  React.useEffect(() => {
    // function
    function handleKeyDown(event) {
      if (event.code === 'Space') {
        // Todo: play or pause
        // use the state setter function to update the state variable
        // pass a function in the state setter to get the current value of the state variable, then do something with it
        // flip the value
        setIsPlaying((currentIsPlaying) =>{
          return !currentIsPlaying;
        });
      }
    }

    // Set the event listener
    window.addEventListener('keydown', handleKeyDown);

    // Cleanup
    return () => {
      window.removeEventListener('keydown', handleKeyDown);
    }
  }, [isPlaying]);

  // Add a second useEffect hook to sync the audio and the state variable isPlaying
  // The one place, to manage the connection between the audio and the state variable
  React.useEffect(() => {
    if (isPlaying) {
      audioRef.current.play();
    } else {
      audioRef.current.pause();
    } 
  }, [isPlaying])

  return (
    <div className="wrapper">
      <div className="media-player">
        <img alt="" src="https://sandpack-bundler.vercel.app/img/take-it-easy.png" />
        <div className="summary">
          <h2>Take It Easy</h2>
          <p>Bvrnout ft. Mia Vaile</p>
        </div>
        <button
          onClick={() => {
            if (isPlaying) {
              audioRef.current.pause();
            } else {
              audioRef.current.play();
            }

            setIsPlaying(!isPlaying);
          }}
        >
          {isPlaying ? <Pause /> : <Play />}
          <VisuallyHidden>Toggle playing</VisuallyHidden>
        </button>

        <audio
          ref={audioRef}
          src={src}
          onEnded={() => {
            setIsPlaying(false);
          }}
        />
      </div>
    </div>
  );
}

export default MediaPlayer;
```

#### The state setter callback

- An important concept, a new way to set React state is the state-setter callback.
- So far in the course, we have been calling the state-setter functions with teh new value:

```JAVASCRIPT
const [count, setCount] = React.useState(0);

// sets 'count' to '100'
setCount(100);
```

- There is an alternative syntax for state updates. We can pass a callback, and React wil invoke that function for us:

```JAVASCRIPT
const [count, setCount] = React.useState(0);

// sets 'count' to '100'
setCount(() => {
  return 100;
});
```

- Whatever we return form this function becomes the new value for teh state, as if we had passed that value directly.
- This is useful because React provides the current value:

```JAVASCRIPT
setCount((currentCount) => {
  return currentCount + 1;
});
```

- 🤔 In most cases, this isn't necessary, since we already have access to the `count` variable. But, when working with effects, it becomes possible for us to lose access to the **freshest version** of a state variable.

- In the exercise above, video player with keyboard, we saw an example of how this alternative syntax can be useful:

```JAVASCRIPT
// Audio Keyboard example
const [isPlaying, setIsPlaying] = React.useState(false);

React.useEffect(() => {
  function handleKeyDown(event) {
    if (event.code === 'Space') {
      setIsPlaying((currentIsPlaying) => {
        return !currentIsPlaying;
      });
    }
  }

  window.addEventListener('keydown', handleKeyDown);

  return () => {
    window.removeEventListener('keydown', handleKeyDown);
  };

  // No dependency on `isPlaying`!
}, []);
```

- If we try to access `isPlaying` inside the `handleKeyDown` callback, that value will be stale, since this effect only runs after the very first mount.
- 📣 By passing a callback function, we pluck the freshest value for this state variable, directly from the component instance.

##### When to use the callback syntax

- Most of the time, we don't' need to use the callback syntax.
- Only necessary when it's possible for state variables to grow stale.

- Stale state variables aren't stricly a `useEfftct` thing. Can happen with other hooks tha thave dependency arrays, like `useMemo` and `useCallback`. Learn about later in module.

- In practice, most of the time a callback syntax is used is because of this situation: need to access a state variable inside an effect, but I don't want to add it to the dependency array.

ℹ️ Why not always use the callback syntax?

For example; a simple counter app:

```JAVASCRIPT
function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button
      onClick={() => {
        setCount((currentCount) => currentCount + 1);
      }}
    >
      {count}
    </button>
  )
}
```

- A callback function works fine, why not use it all the time?
- This is sort of a crutch, by doing this callback function, you don't have to worry about thsi tricky "stale values" stuff. Not recommended to do it this way.
- Recommend, use the default non-callback by default. If you run into bugs, you can try the callback syntax to see if that helps. If it does, try to figure out why. Practice is the only way to get comfortable.
-

#### Exercises, Side Effects

- **Interval counter**

- In this component we want to count how many seonds have elapsed since the component mounted: But the solution has a bug, stops counting at 1 🤔
- Fix the bug so that the number keeps inprementing past 1.

- ACs
- The UI should accurately show the # of seconds since mount.
- There should be no lint warnings, and no `esling-disable` comments.

```JAVASCRIPT
// starter code
import React from 'react';

function Timer() {
  const [count, setCount] = React.useState(0);

  React.useEffect(() => {
    const intervalId = window.setInterval(() => {
      setCount(count + 1);
    }, 1000);

    return () => {
      window.clearInterval(intervalId);
    };
    // eslint-disable-next-line
  }, []);

  return (
    <div className="timer">
      <h1>Seconds since load:</h1>
      <p>{count}</p>
    </div>
  );
}

export default Timer;
```

- **Solution 1:**
- Remove the eslint disable line
- Add the state to the useEffect dependency array
- Semantically, more appropriate to use `setTimeout` since it is mounting and unmounting

```JAVASCRIPT
import React from 'react';

function Timer() {
  const [count, setCount] = React.useState(0);

  React.useEffect(() => {
    const intervalId = window.setTimeout(() => {
      setCount(count + 1);
    }, 1000);

    return () => {
      window.clearTimeout(intervalId);
    };
    
  }, [count]);

  return (
    <div className="timer">
      <h1>Seconds since load:</h1>
      <p>{count}</p>
    </div>
  );
}

export default Timer;
```

- **Solution 2:**
- Use the function callback approach, to get the freshest state variable.
- Also remove the eslint comment

```JAVASCRIPT
import React from 'react';

function Timer() {
  const [count, setCount] = React.useState(0);

  React.useEffect(() => {
    const intervalId = window.setInterval(() => {
      // use a callback function to get the freshest state value
      setCount((currentCount) => {
        return currentCount + 1
        });
    }, 1000);

    return () => {
      window.clearInterval(intervalId);
    };
    
  }, []);

  return (
    <div className="timer">
      <h1>Seconds since load:</h1>
      <p>{count}</p>
    </div>
  );
}

export default Timer;
```

#### Strict Mode

- When you create a new react app using the [boilerplate / meta-framework](https://courses.joshwcomeau.com/joy-of-react/11-tools-of-the-trade/08-boilerplates-and-meta-frameworks) like create-react-app, you will notice something different.

- When we render our app, we wrap it in a `React.StrictMode` element.

```JAVASCRIPT
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

- This element flips a switch inside React, adding in a bunch of restrictions and safeguards designed to improve teh robustness of our app.

- The most confusing part of Strict Mode, is how it impact the `useEffect()` hook.
- It will change how our effects are run. Normally, Component Mounts > Runs the Effect > Add your Event Listener.
- But in Strict Mode, Component Mounts > Run the Effect > Call Cleanup Function > Re-Run the Effect.
- One after the other, almost instantanously, calling it twice.  

- Runs the Effect and calls the function inside the effect. Adds any event listeners then will return any cleanup. Then, it re-runs the Effect function.
- Mount > Run Effect > Run Cleanup > Re-run Effect

- This seems very wasteful, will this make the app twice as slow? Yes, but, it only does this for us developers.
- React has differ modes, development mode, and production mode.
- But when you bundle and build your app for production, Strict mode and any lint warnings are removed.

- Will use Strict Mode from this point on in the course.
- It can be frustrating to use Strict Mode, because it runs the effect twice, can break things.
- 📣 But that's the point, it is highlighting that something is broken. **It can take a subtle bug and highlight to be obvious.**

- If you search on Stack Overflow, Why is my effect running twice, the accepted answer is to remove `<React.StrictMode>` from your index.js

```JAVASCRIPT
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

- But this isn't solving the problem, like removing/disabling eslint warnings. You are just pushing the problem down the line.

- Summary:
- In Normal mode, the sequence of operations:
  - ➡️ Mount
  - ➡️ Run effect

- In Strict Mode, the sequence is:
  - ➡️ Mount
  - ➡️ Run effect
  - ➡️ Run cleanup
  - ➡️ Re-run effect

- Sandbox code example for strict mode:

```JAVASCRIPT
// index.js
import React from "react";
import { createRoot } from 'react-dom/client';
import "./reset.css";
import "./styles.css";
import App from "./App";

const container = document.getElementById('root');
const root = createRoot(container);

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

```JAVASCRIPT
// mediaPlayer.js
import React from 'react';
import { Play, Pause } from 'react-feather';

import VisuallyHidden from './VisuallyHidden';

function MediaPlayer({ src }) {
  const [isPlaying, setIsPlaying] = React.useState(false);

  const audioRef = React.useRef();

  React.useEffect(() => {
    function handleKeyDown(event) {
      if (event.code === 'Space') {
        setIsPlaying((currentIsPlaying) => !currentIsPlaying);
      }
    }

    window.addEventListener('keydown', handleKeyDown);

    return () => {
      // The cleanup work is disabled.
      // Because Strict Mode is enabled (in /index.js),
      // our keyboard shortcut shouldn't work.
      // window.removeEventListener('keydown', handleKeyDown);
    };
  }, []);

  React.useEffect(() => {
    if (isPlaying) {
      audioRef.current.play();
    } else {
      audioRef.current.pause();
    }
  }, [isPlaying]);

  return (
    <div className="wrapper">
      <div className="media-player">
        <img alt="" src="https://sandpack-bundler.vercel.app/img/take-it-easy.png" />
        <div className="summary">
          <h2>Take It Easy</h2>
          <p>Bvrnout ft. Mia Vaile</p>
        </div>
        <button
          onClick={() => {
            setIsPlaying(!isPlaying);
          }}
        >
          {isPlaying ? <Pause /> : <Play />}
          <VisuallyHidden>Toggle playing</VisuallyHidden>
        </button>

        <audio
          ref={audioRef}
          src={src}
          onEnded={() => {
            setIsPlaying(false);
          }}
        />
      </div>
    </div>
  );
}

export default MediaPlayer;
```

#### More information about Strict Mode

- In addition to re-running effects, Strict Mode changes other things. Breaks down to two categories:

1. Safegaurds designed to amplify potential issues
2. Warnings about deprecated APIs

- If you are running a legacy codebase, you may run into deprecatoin warnings.
- All the safeguards follow the same pattern: **running things twice**.

- You can see a full list of Strict Mode changes in the [official documentation](https://react.dev/reference/react/StrictMode).

#### The new default

- Becoming the standard way to use React. Most boiler plate frameworks use Strict Mode by default.
