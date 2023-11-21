# The Joy of React - Module 3 - Course Notes

- [Course Outline Notes](../course-notes.md)

## React Hooks

### Custom Hooks

- React supports custom hooks. You can use the hooks that come along with React, `useState`, `useEffect`
- And we can create our own hooks, like `useInterval` and `useOnScroll`.

- Custom hooks re the best thing about React hooks. That's why everyone like hooks!

#### Not as scary as it sounds

- First thing to understand, not really inveneting our own hooks.

- Sounds scary and only for advaced power users / developers. But think of them as custom hook combos.
- Video Summary:
- The idea, is you can bundle hooks into a custom hook and you can return whatever you want from a custom hook.
- What do you actually need from JSX, what does the component use? That's what you return from the custom hook to make it work.
- In the example below, we need to know the `time` because that's that gets rendered.
- Then insidde the component, you specify `time` to use the custom hook `useTime`

```JAVASCRIPT
import React from 'react';
import format from 'date-fns/format';

function Clock() {
  // Specify to use the custom useTime hook
  const time = useTime();
  
  return (
    <p className="clock">
      {format(time, 'hh:mm:ss a')}
    </p>
  );
}

// Define the custom hook
function useTime() {
  const [time, setTime] = React.useState(
    new Date()
  );
  
  React.useEffect(() => {
    // Effect logic
    const intervalId = window.setInterval(() => {
      setTime(new Date());
    }, 1000);
    
    return () => {
      // Cleanup logic
      window.clearInterval(intervalId);
    };
  }, []);
  // Return the state variable used in the JSX
  return time;
}

export default Clock;
```

- Two advantages to Custom Hooks:
  - Code organization, you can structure stuff however you want.
  - You can share this behaviour between components. You can use the same code `const time = useTime();` The ability to package up React logic and re-use it between components.

📣 A rule we need to follow: Custom Hooks have to start with the word, `use`, so `useTime` or `useInterval` etc.

- React will expect you to start your custom hooks with `use`, and give you eslint warnings.

### Exercises, Custom Hooks

#### useMousePosition

- In an earlier exercise, we tracked the user's mouse position to display it in a `MouseCoords` component.
- Pull this logic into a generic, reusable hook called `useMousePosition`

- Set up a new file, `use-mouse-poistion.js` and put the custom hook into
- Make sure you `return` the state variable
- And don't forget to `export` and `import` your file, via ES6 modules.

```JAVASCRIPT
// App.js
import React from 'react';

// Import the custom hook
import useMousePosition from './hooks/use-mouse-position.js';

// TODO: Pull the mouse-position logic into
// the use-mouse-position.js file!

function App() {
 // Assign the state variable to the hook
 const mousePosition = useMousePosition();

  return (
    <div className="wrapper">
      <p>
        {mousePosition.x} / {mousePosition.y}
      </p>
    </div>
  );
}

export default App;
```

- The seperate file

```JAVASCRIPT
import React from 'react';

function useMousePosition() {
  // TODO
  const [mousePosition, setMousePosition] = React.useState({
    x: 0,
    y: 0,
  });
  
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

  return mousePosition
}

export default useMousePosition;
```

#### useToggle

- In the Digital Clock exercise, we could click a button to toggle the clock off and on:
- Create a `useToggle` hook to update the `showClock` state, but instead of taking a new value, it automatically "flips" to the opposite boolean value.

- **Acceptance Criteria:**

- The `useToggle` hook should use `useState`  internally, to create and manage a state variable.
- We should be able to specify an initial value for the state variable.
- It should return an array containing two items:
  - The state variable
  - A funtion that flips the state variable between `true` and `false`
- Clicking the button should toggle the clock off and on.

- Setup the `use-toggle.js` custom hook

```JAVASCRIPT
import React from 'react';

function useToggle() {
  // Create a generic state hook
  const [value, setValue] = React.useState();

  // define toggle function
  function toggleValue() {
    // call setValue and pass NOT value as argument
    // set from true to false or false to true
    setValue(!value);
  }

  // Return an array
  return [
    // return value
    value,
    // toggle function
    toggleValue
  ]
}

export default useToggle;
```

- Start to use this in `App.js`, update the hook to `useToggle`

```JAVASCRIPT
function App() {
  // TODO: Replace this with “useToggle”!
  const [
    showClock,
    setShowClock
  ] = React.useState(true);

  // rest of component...
}
```

- Add custom hook, `useToggle`, don't forget to remove the `React.` since it's a custom hook.

```JAVASCRIPT
function App() {
  // TODO: Replace this with “useToggle”!
  const [
    showClock,
    toggleClock,
  ] = useToggle(true);

  // rest of component...
}
```

- Update the `onClick` to use the `toggleClock` function.

```JAVASCRIPT
  return (
    <>
      <button
        className="clock-toggle"
        onClick={() => toggleClock()}
      >
        {showClock ? 'Clock ON' : 'Clock OFF'}
      </button>
      
      {showClock && <Clock />}
    </>
  );
```

- Alternative syntax, you can pass the function directly to the `onClick`, passing just the function.

```JAVASCRIPT
  onClick={toggleClock}
```

- Couple more things, set an intial value for the `useToggle` function, and check the typeof value.

```JAVASCRIPT
  // Check the type of for initial value
  if (typeof initialValue !== 'boolean') {
    console.warn('Invalid type for useToggle');
  }
```

- Last thing, update the function to use a callback function to get the most recent value.

```JAVASCRIPT
  function toggleValue() {
    // update to use callback function to always get the fresh value of the state variable and then flip it with logical NOT operator !
    setValue((currentValue) => !currentValue);
  }
```

- Full code example, `useToggle`, custom hook

```JAVASCRIPT
// use-toggle.js
import React from 'react';

function useToggle(initialValue = false) {
  if (
    typeof initialValue !== 'boolean' &&
    typeof initialValue !== 'function'
  ) {
    console.warn('Invalid type for useToggle');
  }

  const [value, setValue] = React.useState(
    initialValue
  );

  function toggleValue() {
    setValue((currentValue) => !currentValue);
  }

  return [value, toggleValue];
}

export default useToggle;
```

```JAVASCRIPT
// App.js
import React from 'react';

import useToggle from './hooks/use-toggle'
import Clock from './Clock'

function App() {
  const [
    showClock,
    toggleClock,
  ] = useToggle(true);

  return (
    <>
      {showClock && <Clock />}
      <button
        className="clock-toggle"
        onClick={toggleClock}
      >
        Toggle clock
      </button>
    </>
  );
}

export default App;
```

#### Exercise, useIsOnscreen

- Similar to the toasty exercise, using the `IntersectionObserver` to trigger a state change when an element enters/exits the viewport. Let's generalize that solution.

- In the solution below, see a large red square that can be scrolled into view. Your job is to fil in the `useIsOnscreen` hook so that we track this element.

- ACs
  - When the red sqaure is in the viewport, we should see the word "YES" in the top right corner.
  - Whent he red square is not in the viewport, we should se the word "NO" instead.
  - We shouldn't use `document.querySelector`.
  - No lint warnings.

- ℹ️ Pure JS version for the intersection observer:

```JAVASCRIPT

Here's the “pure JS” version once again:

  function pureJsVersion() {
    const wrapperElement =
      document.querySelector('.some-class');
  
    const observer = new IntersectionObserver((entries) => {
      const [entry] = entries;
  
      // `entry.isIntersecting` will be true if the
      // element is in the viewport, false if not.
    });
  
    observer.observe(wrapperElement);
  }

To unsubscribe, you can call:

  observer.disconnect();

```

- First Solution:

```JAVASCRIPT
// App.js
import React from 'react';

import useIsOnscreen from './hooks/use-is-onscreen.js';

function App() {

  // Reference to the red box
  const wrapperRef = React.useRef();
  // Pass it as a param to the hook
  const isOnscreen = useIsOnscreen(wrapperRef);

  return (
    <>
      <header>
        Red box visible: {isOnscreen ? 'YES' : 'NO'}
      </header>
      <div className="wrapper">
        <div ref={wrapperRef} className="red box" />
      </div>
    </>
  );
}

export default App;
```

```JAVASCRIPT
// hook, use-is-onscreen.js
import React from 'react';

// Pass in the ref DOM node as a param from App.js
function useIsOnscreen(wrapperRef) {

  // Create a state hook
  const [isOnScreen, setIsOnscreen] = React.useState(false);

  // Setup an effect
  React.useEffect(() => {
    
    // Function, set up the intersection observer
    const observer = new IntersectionObserver((entries) => {
      const [entry] = entries;

      // update the state when called
      // `entry.isIntersecting` will be true if the
      // element is in the viewport, false if not.
      setIsOnscreen(entry.isIntersecting);
  
    });

    // specify the ref, DOM node to watch
    observer.observe(wrapperRef.current);
    
    // Cleanup function, remove the observer
    return () => {
      observer.disconnect();
    }
    
  }, [wrapperRef])
  
  // return the state variable
  return isOnScreen;
}

export default useIsOnscreen;
```

- An alternate version, is who owns the `ref`, should it be in the `app.js` or the `use-is-onscreen.js` hook?
- If you define it in the hook, you will have to define it in the hook, and then return it, so `App.js` can use it.
- In the return, package them up in a array and return.

```JAVASCRIPT
// use-is-onscreen.js
function useIsOnscreen() {
  // Define the ref
  const elementRef = React.useRef();
  
  // Rest of function code
  
  // Package up in array and return both
  return [isOnscreen, elementRef]
}
```

- Then in the `App.js` file, desctructure the array, and call the custom hook.
- Both solution are similar, the only difference, is who owns the ref, in this version, it's owned by the hook, and we pass it along to the `App.js`

- Full Solution:

```JAVASCRIPT
import React from 'react';

import useIsOnscreen from './hooks/use-is-onscreen.js';

function App() {

  // Desctructure the array and call the custom hook
  const [isOnscreen, elementRef] = useIsOnscreen();

  return (
    <>
      <header>
        Red box visible: {isOnscreen ? 'YES' : 'NO'}
      </header>
      <div className="wrapper">
        <div ref={elementRef} className="red box" />
      </div>
    </>
  );
}

export default App;
```

- Custom Hook:

```JAVASCRIPT
// use-is-onscreen.js
import React from 'react';

// Pass in the ref DOM node as a param from App.js
function useIsOnscreen() {

  // Create a state hook
  const [isOnScreen, setIsOnscreen] = React.useState(false);

  const elementRef = React.useRef();

  // Setup an effect
  React.useEffect(() => {
    
    // Function, set up the intersection observer
    const observer = new IntersectionObserver((entries) => {
      const [entry] = entries;

      // update the state when called
      // `entry.isIntersecting` will be true if the
      // element is in the viewport, false if not.
      setIsOnscreen(entry.isIntersecting);
  
    });

    // specify the ref, DOM node to watch
    observer.observe(elementRef.current);
    
    // Cleanup function, remove the observer
    return () => {
      observer.disconnect();
    }
    
  }, [elementRef])
  
  // return the state variable
  return [isOnScreen, elementRef];
}

export default useIsOnscreen;
```

- What if you want to track multiple things in the DOM? That's why we use an array to desctructure, much easier to rename.

```JAVASCRIPT
// App.js
import React from 'react';

import useIsOnscreen from './hooks/use-is-onscreen.js';

function App() {

  // Desctructure the array and call the custom hook
  // Using an array to destructure, allows to rename them to whatever you want
  // Now you can track multiple things using the same hook
  const [isRedOnscreen, redElementRef] = useIsOnscreen();
  const [isPurpleOnscreen, purpleElementRef] = useIsOnscreen();

  return (
    <>
      <header>
        Red box visible: {isRedOnscreen ? 'YES' : 'NO'}
        <br />
        Purple box visible: {isPurpleOnscreen ? 'YES' : 'NO'}
      </header>
      <div className="wrapper">
        <div ref={redElementRef} className="red box" />
        <div ref={purpleElementRef} className="purple box" />
      </div>
    </>
  );
}

export default App;
```
