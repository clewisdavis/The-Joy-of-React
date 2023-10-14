# Module 2 - Working with State

- [Course Outline Notes](course-notes.md)

## Event Handlers

- When a user interacts with the page, hundreds of events are fired off in response. The browser is tracking every little thing you do.

- These events are important when building dynamic applications, we will listen for these events and use them to trigger state changes.
- When the use clicks on "X" button, we dismiss teh prompt, when user submits the form, we show a loading spinner.

- In order to respond to an event, we need to listen for it. JS already provides a built in way to do this, `addEventListener` method:

```JAVASCRIPT
const button = document.querySelector('.btn');

function doSomething() {
  // stuff here
}

button.addEventListener('click', doSomething);
```

- In the code above, we are listening for a specific event `click` targeting a specific element `btn`. We have a function to handle this event `doSomething`.
- When the user clicks on this particular button, your handler function will be invoked, allowing us to do something in response.

- The web platform offers another way to do this. We can embed our handler right in the HTML

```HTML
<button onclick="doSomething()">
  Click me!
</button>
```

- Rect piggybacks on this pattern, allowing us to pass an event handler right into the JSX:

```JAVASCRIPT
function App() {
  function doSomething() {
    // Stuff here
  }

  return (
    <button onClick={doSomething}>
      Click me!
    </button>
  )
}
```

- As with the `addEventListener`, this code will do the same thing: when use clicks on the button, the `doSomething` function is called.

- **This is the recommended way to handle events in React.**
- Try and use the "onX" props like `onClick` and `onChange` when possible.

- Few reasons why:
  - **Automatic cleanup:** When we use an event listener, we supposed to remove it when we are done, with `removeEventListener`. If we forget to do this, we introduce a memory leak. React automatically removes listeners when we use 'on X' handler functions.
  - **Improved performance:** React can optimize things for use, like batching multiple event listeners together to reduce the memory consumption.
  - **No DOM interaction:** Avoid interacting with the DOM directly.

- ℹ️ One of the core ideas behind React, is that is does the DOM manipulation for you. Stay within React's abstraction rather than trying to compete with it to manage the DOM.

#### Differences from HTML, Events

- Looking at the `onChange` prop, it looks very similar tot he in HTML method of adding event handlers.

##### Camel Casing

- In JSX, you have to write `camelCased` attribute names in JSX.
- Be careful to write `onClick` instead of `onclick`. Other examples, `onChange`, `onKeyDown`, `onTransitionEnd` etc.
- If you forget, React will let you know.

##### Passing a function reference

- When working with event handlers in React, we need to pass a reference to the function. We don't call the function like we do in HTML:

```HTML
// ✅ We want to do this:
<button onClick={doSomething} />

// 🚫 Not this:
<button onClick={doSomething()} />
```

- When we include the parentheses, we invoke th function right away, the moment the React app is rendered. We want to give React a reference to the function, so React can call it at a later time, when the user clicks on the button.

#### Specifying Arguments

- Here is the thing, what if we want to specify an argument to a function?

- For example, if you wanted to make a function called `setTheme`, and we use it to change the user's color theme from Light Mode, to Dark Mode.

- To do this, we need to supply he name of the theme we are switching to, like this:

```JAVASCRIPT
// Switch to light mode:
setTheme('light');

// Switch to dark mode:
setTheme('dark');
```

- How can we do this? If we pass `setTheme` as a reference to the `onClick`, we can't supply arguments:

```JAVASCRIPT
// 🚫 Invalid: calls the function without 'dark'
<button onClick={setTheme}>
  Toggle theme
</button>
```

- In order to specify the argument, we need to wrap it in a parentheses, but it gets called right away:

```JAVASCRIPT
// 🚫 Invalid: calls the function right away
<button onClick={setTheme('dark')}>
  Toggle theme
</button>
```

- We can solve this problem with a wrapper function

```JAVASCRIPT
<button onClick={() => setTheme('dark')}>
  Toggle Theme
</button>
```

- We are creating a new anonymous arrow function, `() => setTheme('dark')` and passing it to React, to be called when the user clicks the button.

- When the button is clicked, the function is called and the code inside the function is executed: `setTheme('dark')`.

- For a refresher, check out the [Arrow Function Primer](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/04-arrow-functions) lesson 👀

#### Exercises, Event Handlers

##### Click the ball, build a simple game

- The goal of the game is to click the ball, to show an alert when the ball is clicked:

- We can us the `window.alert()` to show the message.
- AC's
  - When the user click on the ball, a winning message should be shown.
  - You should handle 'click' events specifically, as this even is triggered on click, on tap, or even when a user focuses on teh element with the keyboard and hits "Enter" Key
    - If you don't use a pointer device, you can use the keyboard method to test your code.

```JAVASCRIPT
import React from 'react';

import VisuallyHidden from './VisuallyHidden';

function ClickBallGame() {
  return (
    <div className="wrapper">
      <button
        className="ball"
      >
        <VisuallyHidden>
          Ball
        </VisuallyHidden>
      </button>
    </div>
  );
}

export default ClickBallGame;
```

- Solution:
- ℹ️ You can write as a separate function, especially is you have multiple expressions inside your function.

```JAVASCRIPT
import React from 'react';

import VisuallyHidden from './VisuallyHidden';

function ClickBallGame() {
  function handleClick() {
    window.alert('You win');
  }
  return (
    <div className="wrapper">
      <button
        className="ball"
        onClick={handleClick}
      >
        <VisuallyHidden>
          Ball
        </VisuallyHidden>
      </button>
    </div>
  );
}

export default ClickBallGame;
```

- You can also do this inline function, to achieve the same result.

```JAVASCRIPT
  return (
    <div className="wrapper">
      <button
        className="ball"
        onClick={() => window.alert('You win')}
      >
        <VisuallyHidden>
          Ball
        </VisuallyHidden>
      </button>
    </div>
  );
```

##### Click the ball v2

- Now, in addition to the ball, it now has a bomb. If the bomb is clicked, we want to show a 'lose' message.
- The problem with the started code, clicking on either the item, the ball or bomb, it shows the 'lose' message.

- You mission, if you choose it, it to fix the code so that i throws the right message depending on which item is clicked.

- AC's
  - When the user clicks the ball, a winning message should be shown.
  - When the user clicks the bomb, a losing message should be shown.
  - The `handeClick` function should still be used, and you shouldn't have to change anything about the function itself.

```JAVASCRIPT
// STARTER CODE
import React from 'react';

import VisuallyHidden from './VisuallyHidden';

function ClickBallGame() {
  function handleClick(type) {
    if (type === 'win') {
      alert('You win!');
    } else {
      alert('You lose :(');
    }
  }
  
  return (
    <div className="wrapper">
      <button
        className="ball"
        onClick={handleClick}
      >
        <VisuallyHidden>
          Ball
        </VisuallyHidden>
      </button>
      <button
        className="bomb"
        onClick={handleClick}
      >
        <span
          role="img"
          aria-label="bomb"
        >
          💣
        </span>
      </button>
    </div>
  );
}

export default ClickBallGame;
```

- My solution:
- Put a wrapper function on the `onClick` and pass in the `type` parameter into the `handleClick()` function.

```JAVASCRIPT
  return (
    <div className="wrapper">
      <button
        className="ball"
        onClick={() => handleClick('win')}
      >
        <VisuallyHidden>
          Ball
        </VisuallyHidden>
      </button>
      <button
        className="bomb"
        onClick={() => handleClick('lose')}
      >
        <span
          role="img"
          aria-label="bomb"
        >
          💣
        </span>
      </button>
    </div>
  );
```
