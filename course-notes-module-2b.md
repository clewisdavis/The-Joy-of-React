# Module 2 - Working with State

- [Course Outline Notes](course-notes.md)

## The useState Hook

- Below is a common example of a minimal counter demo.
- Click on the button and watch the count increase:

```JAVASCRIPT
import React from 'react';

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Value: {count}
    </button>
  );
}

export default Counter;
```

- Break this down, what is going on here:
- Our goal is to keep track of the number of times the user has clicked the button.
- For dynamic values, we need to use React state.
- State is used for values that change over time.

- To create a state variable, we use the `useState` function.
- This function takes a single argument: the initial value. In this case, that value is `0`. The value is chosen because when the page first loads, we clicked the button 0 times.

- `useState` is a **hook**. A hook is a special type of function that allows us to 'hook into' React internals.

- The `useState` hook returns an array containing two items:

1. The current value of the state variable. We've decided to call it `count`, this can be whatever you want to name it.
2. A function we can use to update the state variable. We named it `setCount`. It's best practice to pre-pend the `set` to the function.

- ℹ️ Destructing Assignment, below is the same thing as the above `const [count, setCount]` we are destructuring `count` and `setCount`.

```JAVASCRIPT
const countArray = React.useState(0);

const count = countArray[0];
const setCount = countArray[1];
```

- 👀 Can check out the [Array Destructuring Primer Lesson](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/06-array-destructuring)

##### Naming Conventions

- When we create a state variable, we can name it whatever we want.
- However, it's best practice to follow the `x`, `setX` convention:

```JAVASCRIPT
const [user, setUser] = React.useState();
const [errorMessage, setErrorMessage] = React.useState();
const [flowerBouquet, setFlowerBouquet] = React.useState();
```

- `user` The first destructured variable si the name of the thing we are tracking.
- `setUser` The second variable prefixes that name with the `set`, specifying it's a function that can be called to change the thing. This is sometimes referred to as a "setter function", since it sets the new value of teh state variable.

- ℹ️ Importing the hook?
  - Some tutorials write this code differently.
  - Import at the top of the page. `import React, { useState } from 'react';`
  - Instead of `React.useState`, you can import `useState` function at the top of the file and using it on it's own, `useState`.

##### Initial Value

- React state variables can be given an initial value:

```JAVASCRIPT
const [count, setCount] = React.useState(1);
console.log(count);
```

- We can also supply a function. React will call this function on the very first render to evaluate the initial value:

```JAVASCRIPT
const [count, setCount] = ReactuseState(() => {
  return 1 + 1;
});

console.log(count); // 2
```

- Calling a function can be useful to do an expensive operation to calculate the initial value. For example, reading from localStorage:

```JAVASCRIPT
const [count, setCount] = React.useState(() => {
  return window.localStorage.getItem('saved-count');
});
```

- The benefit here is that we are only doing the expensive work (redon from localStorage) once, on the initial render, rather than doing it on every single render.

- In a later lesson, discuss localStorage

- ℹ️ Hold up, What's the difference between those two?

```JAVASCRIPT
// Form 1:
const [count, setCount] = React.useState(
  window.localStorage.getItem('saved-count')
);

// Form 2:
const [count, setCount] = React.useState(() => {
  return window.localStorage.getItem('saved-count');
});
```

- Helpful to forget about React for a moment and think in pure JS.

- Consider this function:

```JAVASCRIPT
function run() {
  console.log('Hello');
}
```

- When we call the `run` function, we will log the word "Hello", if I call the function 5 times, I get 5 "Hello" in the console.

- Tweak it:

```JAVASCRIPT
function run() {
  const sayHi = () => {
    console.log('Hello');
  };
}
```

- Now, when I call the `run` function, nothing is logged to the console. That's because the `console.log` is wrapped up in an inner function, `sayHi`.
- every time we call the `run` function, we are creating a brand new `sayHi` function, but never calling it.

#### Core React Loop

- We have seen, when we update the state variable by calling the setter function `setCount` the UI gets updated. But how does this actually work?

- This is the core of React, literally named for how it reacts to state changes.

- Let's use the counter as an example:

```JAVASCRIPT
function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Value: {count}
    </button>
  )
}
```

- What is happening here exactly when the component is rendered for the first time?
- Our `Counter` function returns a bunch of JSX, Let's look at it in pure JS, so see what's going on.

```JAVASCRIPT
function Counter() {
  const [count, setCount] = React.useState(0);

  return React.createElement(
    'button',
    { onClick: () => setCount(count + 1) },
    'Value: ',
    count
  );
}
```

- When this code runs, `React.createElement` produces a React element, which is a plain JS object like this:

```JAVASCRIPT
{
  type: 'button',
  key: null,
  ref: null,
  props: {
    onClick: () => setCount(count + 1),
  },
  children: 'Value: 0',
  _owner: null,
  _store: { validated: false }
}
```

- As we learned before, React elements are essentially descriptions of the UI we want. Here we are saying we want a button that contains the text "Value: 0".
- We could visualize this JS object as the following HTML snippet:

```HTML
<button>
  Value: 0
</button>
```

- Our React element, that JS object, is describing teh DOM structure. React takes that description and turns it into a real thing.
- It creates a `<button>` DOM node and put on the page.

- I didn't show the `onClick` handler in this little sketch, but it's very much a part of this process. When react creates and injects the `<button>` DOM node, it attaches our handler function.

- **Let's think about what happens when the button is clicked.**
- The `setCount` function will be called and we will pass in a new value. `count` will be incremented from 0 to 1.

- **Whenever a state variable is updated, it triggers a re-render.**
- Once again, React will cal the `Counter` function.
- This creates a brand-new React element, a new description of the UI we want.

- The new react element describes this DOM structure:

```HTML
<button>
  Value: 1
</button>
```

- **Each render is like taking a snapshot.**
- We generate a description that shows what the UI should look like, based on the components props/state. Like a photo that captures what things were like at a moment in time.
- And React has two snapshots:

```HTML
<button>
  Value: 0
</button>
```

```HTML
<button>
  Value: 1
</button>
```

- The user clicked a button and this second snapshot was generated.
- React now has to figure out how to update the DOM, so that is matches this latest snapshot.
- It's like those puzzle games, where you have to figure out the differences between two images.
- React has to play this sort of game, looking for the changes between teh two snapshots.

- **This process is known as reconciliation.**
- React figures out what's changed and updates the UI.
- Once React figures out the difference, it will need to **commit** these changes. With precision, it updates teh DOM, taking care only to tweak the things hat need to be tweaked.

In our button case, the operation would be something like:

```JAVASCRIPT
button.innerText = "Value: 1";
```

- **This is the fundamental "flow" of React, the core loop.**
- The sequence is like:

- Mount
  - Trigger > Render > Commit

- Mount: When we render the component for the first time, there is no previous snapshot to compare it to. So, React will create al the necessary DOM nodes from scratch, and inject them into the page.

##### Rendering vs. Painting

- Like 3D software, when you take raw data and turn it into an image, like a 3D model in Blender.
- The term "render" generally refers to this sort of thing; we are taking some sort of unprocessed raw input, and generating a final ready to use output.

- Even in web frameworks, the definition is pretty consistent.

- In React, however the term "render" means something slightly different. I think so much of the confusion in React comes from this misunderstanding.

- In this example:

```JAVASCRIPT
function AgeLimit({ age }) {
  if (age < 18) {
    return (
      <p>You're not old enough!</p>
    );
  }

  return (
    <p>Hello, adult!</p>
  );
}
```

- Our `AgeLimit` component check an `age` prop and returns one of two paragraphs.
- Now, let's suppose we re-render this component, and wind up with the following before/after pair of snapshots:

```JAVASCRIPT
age: 16

{
  type: 'p',
  key: null,
  ref: null,
  props: {},
  children: "You're not old enough!",
}
```

```JAVASCRIPT
age: 17

{
  type: 'p',
  key: null,
  ref: null,
  props: {},
  children: "You're not old enough!",
}
```

- In both cases, `age` is less than 18, and so we wind up with teh exact same UI.
- As a result, *no DOM mutation* happens at all.

- When we talk about 're-rendering', we are saying to check it anything's changed.
- 💡 If we spot a difference between snapshots, React will need to update the DOM, but it will be a precisely-targeted minimal change. 💡

- When React does change a part of the DOM, teh browser will need to re-paint. A re-paint is when the pixels on the screen are re-drawn because a part of the DOM was mutated.

- This is done natively by the browser when teh DOM is edited with JS, whether by React, Angular, jQuery, vanilla JS, etc.

To Summarize: 🚀 React Re-Rendering

- A **re-render** is a React process where we figure out what needs to change, AKA reconciliation, to spot the difference between snapshots.

- If something has changed between eh two snapshots, React will "commit" those changes by deleting the DOM, so that it matches the latest snapshot.

- Whenever a SOM node is edited, the browser will re-paint, re-drawing the relevant pixel so that the user sees the correct UI.

- **No all re-renders require re-paints!** If nothing has changed between snapshots, React won't edit any DOM nodes, and nothing will be re-painted.

- 👀 The critical thing to understand si that when we talk about re-rendering, we are not saying that we should throw away the current UI and re-build everything from scratch.

- React ties to keep the re-painting to a minimum, because re-painting is slow. Instead of generating a bunch of new DOM notes from scratch (lots of painting), it figures out what's changed between snapshots and make the required tweaks with surgical precision.

- For more in-depth; see react docs [Render and Commit](https://react.dev/learn/render-and-commit)

#### Asynchronous Updates

- Consider the following code: What value would you expect to see in the developer console when the user clicks the button for the first time?

```JAVASCRIPT
function App() {
  const [count, setCount] = React.useState(0);

  return (
    <>
      <p>
        You've clicked {count} times.
      </p>
      <button
        onClick={() => {
          setCount(count + 1);
          console.log(count)
        }}
      >
        Click me!
      </button>
    </>
  );
}
```

- The answer is 0, I guessed 1, what is going on here?

- When we create our state variable, we initialize ti to `0`. Then, when we click the button, we increment it by 1, to `1`. So shouldn't it log `1` and not `0`?

- Here is the catch: **state setters are not immediate.**

- When we call `setCount` we tell Rect that we would like to request a change to a state variable.
- React does not immediately drop everything; it waits until the current operation is completed (processing the click), and then updates the value and triggers a re-render.

- For now, the thing to remember is that **updating a state variable is asynchronous**. It affects what the state will be for the **next render**. It's a scheduled update.

- Here is how to fix the code, so that we have access to the new value right away:

```JAVASCRIPT
function App() {
  const [count, setCount] = React.useState(0);

  return (
    <>
      <p>
       You clicked {count} times.
      </p>
      <button
        onClick={() => {
          const nextCount = count + 1;
          setCount(nextCount);
          console.log(nextCount)
        }}
      >
        Click me!
      </button>
    </>
  )
}
```

- We store the updated value in a variable, so that we can access that variable whenever we want to know what the new value is.

- I like to use prefix `next`, since it conveys that we are talking about the 'future' value of the state, what it will be on the next render. But you can name whatever you want.
- Try and fix the bug:

#### Exercises, useState Hook, Role Playing Game

- You are building an in-browser role playing game, but we have a bug. The wrong values are being shown to the user when the character levels up.

```JAVASCRIPT
import React from 'react';

function Character() {
  const [strength, setStrength] = React.useState(6);
  const [dexterity, setDexterity] = React.useState(9);
  const [intelligence, setIntelligence] = React.useState(15);

  function triggerLevelUp() {
    setStrength(strength + 1);
    setDexterity(dexterity + 2);
    setIntelligence(intelligence + 3);

    window.alert(`
      Congratulations! Your hero now has the following stats:
      Str: ${strength}
      Dex: ${dexterity}
      Int: ${intelligence}
    `);
  }

  return (
    <div className="wrapper">
      <img
        alt="8-bit wizard character"
        src="https://sandpack-bundler.vercel.app/img/mage-sprite.gif"
      />
      <button onClick={triggerLevelUp}>
        Level Up
      </button>
    </div>
  );
}

export default Character;
```

- My solution: To declare variables for the values so they update right away:
- What is going on with this code?
- Why are we seeing stale values when you click on the 'Level Up' button?
- It's not actually changing the variable as you would expect, `strength = strength + 1`
- The way the `triggerLevelUp()` function works in state, is they schedule an update.
- It's a way to tell React, hey we are changing some stuff, increase this value by 1.
- So please, re-render with this new value.
- Telling React to redo all the work that it has already done, to determine what the UI is.
- And on the next pass, next render, it should use the new values.
- When React sees `setStrength(strength + 1);`, React makes a "calendar appointment" but *it doesn't interrupt what it is already doing*, running through the function.
- **It's going to go through the rest of the function, without changing anything.**

- And the next time we run the function `Character`, the new values will be populated.

```JAVASCRIPT
function Character() {
  const [strength, setStrength] = React.useState(6);
  const [dexterity, setDexterity] = React.useState(9);
  const [intelligence, setIntelligence] = React.useState(15);

  function triggerLevelUp() {
    const nextStrength = strength + 1;
    const nextDexterity = dexterity + 2;
    const nextIntelligence = intelligence + 3;
    
    setStrength(nextStrength);
    setDexterity(nextDexterity);
    setIntelligence(nextIntelligence);

    window.alert(`
      Congratulations! Your hero now has the following stats:
      Str: ${nextStrength}
      Dex: ${nextDexterity}
      Int: ${nextIntelligence}
    `);
  }
```

#### Exercise, Counter 2.0

- The buttons have already been added to the page, but they don't do anything yet. The task is to wire up the buttons with the following ACs.

ACs:

- The up arrow button should increase the count by 1.
- The double arrow up button should increase the count by 10.
- The refresh button should reset the count to 0.
- The pound button should set the count to a random number between 1 and 100.
- The double down arrow button should decrease the count by 10.
- The down arrow button should decrease the count by 1.

- Starter code:

```JAVASCRIPT
import { ChevronUp, ChevronsUp, ChevronDown, ChevronsDown, RotateCcw, Hash } from 'react-feather'

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <div className="wrapper">
      <p>
        <span>Current value:</span>
        <span className="count">
          {count}
        </span>
      </p>
      <div className="button-row">
        <button>
          <ChevronUp />
          <span className="visually-hidden">
            Increase slightly
          </span>
        </button>
        <button>
          <ChevronsUp />
          <span className="visually-hidden">
            Increase a lot
          </span>
        </button>
        <button>
          <RotateCcw />
          <span className="visually-hidden">
            Reset
          </span>
        </button>
        <button>
          <Hash />
          <span className="visually-hidden">
            Set to random value
          </span>
        </button>
        <button>
          <ChevronsDown />
          <span className="visually-hidden">
            Decrease a lot
          </span>
        </button>
        <button>
          <ChevronDown />
          <span className="visually-hidden">
            Decrease slightly
          </span>
        </button>
      </div>
    </div>
  );
}

export default Counter;
```

- The solution: use the `setCount` **state setter function** to increase and decrease.

```JAVASCRIPT
import React from 'react';
import { ChevronUp, ChevronsUp, ChevronDown, ChevronsDown, RotateCcw, Hash } from 'react-feather'

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <div className="wrapper">
      <p>
        <span>Current value:</span>
        <span className="count">
          {count}
        </span>
      </p>
      <div className="button-row">
        <button
          // Increase by 1
          onClick={
            () => setCount(count + 1)
          }
        >
          <ChevronUp />
          <span className="visually-hidden">
            Increase slightly
          </span>
        </button>
        <button 
          // Increase by 10
          onClick={
            () => setCount(count + 10)
          }
        >
          <ChevronsUp />
          <span className="visually-hidden">
            Increase a lot
          </span>
        </button>
        <button
          onClick={
            () => setCount(0)
          }
        >
          <RotateCcw />
          <span className="visually-hidden">
            Reset
          </span>
        </button>
        <button
          // set random value
          onClick={
            () => setCount(Math.ceil(Math.random() * 100))
          }
        >
          <Hash />
          <span className="visually-hidden">
            Set to random value
          </span>
        </button>
        <button
          //decrease by 10
          onClick={
            () => setCount(count - 10)
          }
        >
          <ChevronsDown />
          <span className="visually-hidden">
            Decrease a lot
          </span>
        </button>
        <button
          // decrease by 1
          onClick={
            () => setCount(count - 1)
          }
        >
          <ChevronDown />
          <span className="visually-hidden">
            Decrease slightly
          </span>
        </button>
      </div>
    </div>
  );
}

export default Counter;
```

#### Why the dance?

- Why so much to manage state management in React?
- React is based on the confines of JavaScript.
- JS doesn't give us a way to observe variables.
- We need to know, when a variable changes in order for React to trigger a re-render.
- I our counter example, the only way for the UI to update, is for all the code to re-run. It doesn't automatically re-run the code when you change a variable.

```JAVASCRIPT
import React from 'react';

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Value: {count}
    </button>
  );
}

export default Counter;
```

- How do you get the increment, if you try in plain JS. You can't, it doesn't persist across function calls.

```JAVASCRIPT
function helloWorld() {
  let count = 0;
  count = count + 1;
}

helloWorld(); // 1
helloWorld(); // 1
helloWorld(); // 1
helloWorld(); // 1
```

- every time you call that function, you get the same results, it doesn't increment. We re-initialize the variable and increment by one.
- The increment `count + 1` doesn't persist across function calls. JS doesn't give us a way to thread values through function calls.

- Back to React, **Components in React are Functions in JS.**
- If you think through what the `Counter()` function above does.

```JAVASCRIPT
// Start you app
ReactDOM.render(<App />)
// Transpile to
ReactDOM.render(React.createElement(App))
// Calling this function, get an JS object with all the stuff
{ type: App, props {}}
// The point is, React is going to evoke the App() function
// Re-render
App()
// Re-render
App()
// Re-render
App()
// Re-render
App()

```

- The point is, React is going to evoke the `App()` function, every time we trigger a re-render.
- every time you re-render, it's like a timeline, a snapshot of that component.
- The variable needs to be defined within the component, so every instance can have it's own dynamic state.

- React exist in the confines of JS, and if we define a variable within a component/function, then that variable will be re-initialized every time we call the function.

```JAVASCRIPT
function App() {
  let count = 0;
  return {
    <p>{count}</p>
  }
}
```

- Back to `React.useState(0);`, we need to have some way, to access the current value of count, without redefining it in every function call.
- The `React.useState()` function is reaching into React, and pulling out the state variable.
- On the very first render, we are giving React the value, `const [count, setCount] = React.useState(0);` and putting it into the `count` variable.
- On the second render, we are reaching in and getting the new value.
- And so on, we are pulling the value out of React, which exist outside the scope of the function.

- The `setCount` function, JS doesn't give us a way to observe values, so we need to tell React it's time to render.
- The `setCount` is doing two things:
  - Primary thing, we are changing the value, and scheduling the update, so when we re-render the function, the `count` variable will have the new value.
  - Secondary thing, we are also signaling to React it's time to re-render.

- The easiest way to think about React, is a series of snapshots over time.
- When you first call a component, `App()` you get an initial value.
- Then every time you call the component after that, you get another snapshot and on and one.
- For each of these snapshot, the `state` variable is being populated within React.

- React doesn't compile to some other language, it is always restricted by the rules of JS.
