# The Joy of React - Module 3 - Course Notes

- [Course Outline Notes](../course-notes.md)

## React Hooks

## Memoization

- In large React apps, re-render can happen a lot.
- Can lead to some performance issues over time.
- Dig deeper into why React components re-render. And look at React tools to optimize this.

- The `React.memo` component wrapper
- The `useCallback` hook
- The `useMemo` hook

### Why React Re-Renders

- First, get comfortable with the React render cycle.
- We've seen how we can call a state setter function (`setCount`, `setUser`) to trigger a re-render.
- React captures a snapshot of what the UI should look like given this new value for a state variable.

- ℹ️ Rendering vs. Painting - Not the sme thing
  - Re-render, React process where it figures out what needs to change (reconciliation, spot the difference)
  - Painting, whenever a DOM node is edited, the browser will re-paint, redrawing the UI.

#### The Basic Rule - Re-Renders - Core React Loop

- The fundamental truth: **Every re-render in React starts with a state change.**
- It's the trigger in React for a comoponent to re-render.
- But, don't component re-render when their props change?

- Here's the thing, when a component re-renders, **it also re-render all of its decendants.**

- Let's look at an example; Counter app.

```JAVASCRIPT
// counter app example, React Re-Render
import React from 'react';

function App() {
  return (
    <>
      <Counter />
      <footer>
        <p>Copyright 2022 Big Count Inc.</p>
      </footer>
    </>
  );
}

function Counter() {
  const [count, setCount] = React.useState(0);
  
  return (
    <main>
      <BigCountNumber count={count} />
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </main>
  );
}

function BigCountNumber({ count }) {
  return (
    <p>
      <span className="prefix">Count:</span>
      {count}
    </p>
  );
}

export default App;
```

- In this example, we have 3 components, `App` at the top, which renders `Counter`, which renders `BigCountNumber`.
- In React, every state variable is attached to a particular component instance.
- In this example, we have a single piece of state, `count`, which is associated with the `Counter` component.
- Whenever thsi state change, `Counter` re-renders. And because `BigCountNumber` is being rendered by `Counter`, it will re-render as well.

- Graph of what is happening:

![Alt text](images/image-15.png)

- 📣 Big Misocnception #1 - **The entire app re-renders whenever a state variable changes.**

- Some believe, that every state cahnge in React forces an application wide render. **This isn't true.**
- Re-renders only affect the component that owns the state + it decendants (if any).
- In the above example, `App` component doesn't have to re-render when the count state variable changes.

- React "main job" it so keep the application UI in sync with teh react state. The point of a re-render is to **figure out what needs to change**.

- Consider the "Counter" example above. When the application first mounts, React renders all of our component and comes up with the following sketch for what the DOM should look like.

```JAVASCRIPT
    <main>
      <p>
        <span class="prefix">Count:</span>
        0
      </p>
      <button>
        Increment
      </button>
    </main>
    <footer>
      <p>Copyright 2022 Big Count Inc.</p>
    </footer>
```

- When the user clicks on the button, the `count` state variable flips from `0` to `1`. How does this affect the UI?
- That's what we hope to learn from doing another render!

- Rect re-runs code for the `Counter` and `BigCountNumber` compoennts, and we generate a new picture of the DOM we want:

```JAVASCRIPT
<main>
  <p>
    <span class="prefix">Count:</span>
    1
  </p>
  <button>
    Increment
  </button>
</main>
<footer>
  <p>Copyright 2022 Big Count Inc.</p>
</footer>
```

- 📣 **As we've learned, each render is like a snapshot,** a photo that tells us what the UI should look like, based on the current application state.

- React then plays a "find the differences" game to figure out what's changed between snapshots.
- I this case, it seems our paragraph has a text node that changed from `0` to `1`, and so it edits the text node to match the snapshot.
- Satisfied that its work is done, React settles back and waits for the next state change.
- 📣 **This is what we have called the core React loop.**

- Render Graph:

![Render Graph](images/image-16.png)

- Our `count` state is associated with the `Counter` component. Because data can't flow "up" in a React application, we know that this state change can't possibly affect `<App />`. And so we dont' need to re-render hat component.

- But we do need to re-render `Counter`'s child, BigCountNumber.
- This is the compoennt that actaully displays the `count` state.
- If we don't render it, we won't knwo that our paragraph's text node should change from `0` to `1`.
- We ened to include this component in our sketch.  

- 📣 The point of a re-render is to figure out how a state change should affect the user UI. And we need to re-render all potentailly-affected components, to get an accurate snapshot. 📣

#### It's not about the props

- Now, big misconception #2: **A component will re-render because its props change.**

- An updated example:
- In the code below, our "Counter" app has been given a brand new component, `Decoration`

```JAVASCRIPT
import React from 'react';

import Decoration from './Decoration';
import BigCountNumber from './BigCountNumber';

function Counter() {
  const [count, setCount] = React.useState(0);
  
  return (
    <main>
      <BigCountNumber count={count} />
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
      
      {/* 👇 This fella is new 👇 */}
      <Decoration />
    </main>
  );
}

export default Counter;
```

- Our counter now has a `Decoration` component. It doesn't depend on `count`, so it probably won't re-render when `count` changes, right? Not quite.

![re-render](images/image-17.png)

- When a compoennt re-renders, it tried to re-render al descendants, regardless of whether they being passed a particular state varuable through props or not.

- Counter-intuitive, if we are not passing `count` as a prop to `<Decoration>`, why would it need to re-render?
- The answer: it's hard for React to know, with 100% certainty, whether `<Decoration>` depends, directly or indirectly, on the `count` state variable.

- In an ideal world, React components would always be pure. Always produces the same UI when given the same props.

- In the real world, many of our components are impure. For example;

```JAVASCRIPT
function CurrentTime() {
  const now = new Date();

  return (
    <p>It is currently {now.toString()}</p>
  );
}
```

- This component will display a different value whenever it's rendered, since it relies on the current time `Date()`.
- **React's #1 goal** is to make sure that the UI the user sees is kept "in sync" with the application state.
- So, React will err on the side of to many renders. It doesn't want to risk showing the user a stale UI.

- Going back to the misconception: props have nothing to do with re-renders.
- `<BigCountNumber>` component is not re-rendering because the count prop changed.

- 📣 When a component re-renders, because one of tis state variables has been updated, *that re-render will casecade all the way down the tree*, in order for React to figure out what the new snapshot should look like.

- Saying that, we can optimize the process.

### Pure Components

- In the last lesson, we saw how a component will automatically re-render when its parent re-renders, regardless of whether or not its props have changed.

- In larger apps, this can lead to performance issues. A single state change might re-render dozens or even hundres of components, even if only a small fraction of them actaully need to re-render.

- React provides an escape hatch we can use: the `React.memo` utility.

```JAVASCRIPT
  function Decotation() {
    return (
      <div className="decoration">
        🍰
      </div>
    );
  }

  const PureDecoration = React.memo(Decoration);

  export default PureDecoration;
```

- React.memo is a utility that lets us tell React: "Hey, this component is pure! It doesn't need to re-render unless its props or state changes. It will always return the exact same UI when given the same props + state".

- It takes a component as an argument (`Decoration`) and augments it, giving it a new super power: **it can selectively ignore re-renders that don't affect it.**

- When the parent component re-renders, React will try to re-render the child `PureDecoration`, but `PureDecoration` steps in and says "None of my props ahve changed, and so I won't be re-rendering this time"

- 📣 This uses a technique known as **memoization**.

- It's missing the R, but think of it as "momorization". The idea React will remember the previosu snapshot.
- If none of the props have chnaged, rec will re-use that stale snapshot rather than going through the trouble of generating a brand new one.

- Let's suppose I wrap both `BigCountNumber` and `Decoration` with the `React.memo` helper.
- Here is how would affect the renders:

![Alt text](images/image-18.png)

- When count changes, we re-render Counter, and React will try to render both descendant components.

- Because `BigCountNumber` takes `count` as a prop, and because that prop has changed, `BigCountNumber` is re-rendered.
- But because Decorations's props haven't changed (doesn't have any) the original snapshot is used.

- Pretend that React.memo is like a lazy photographer. If you ask it to take 5 photos of the exact same thing, will take 1 photo and give you 5 copies of it. The photographer will only snap a new picture when your instructions change.

- [Live code example; Use Memo](https://codesandbox.io/s/ynbfuv?file=/Counter.js&utm_medium=sandpack)

- To summerize:

- The only way to re-render anythign in react si to update a state variable by calling a state-setter function (eg. `setCount`).
- When a component re-renders, it automatically re-renders all of its descendants, even if none of their props have changed.
- We can wearp our component with React.memo to optimize it, so that it only re-renders if at least 1 of its props have changed since the last render.

- ℹ️ Why isn't this the default behaviour?
- Easy to overestimate how expensive re-renders are. In teh case of our `Decoration` component, re-renders are lighting quick.
- If a component has a bunch of props and not a lot of descendants, it can actaully be slower to check if any of the props have changed compared to re-rendering the component.
- The official recommendation is to use React.memo as-needed, when trying to fix a perfomance issue. No need to apply pre-emptively.

### The useMemo Hook

- In the last lesson we saw how `React.memo` helper let sus memoize a component, so that it only re-renders when its props/state changes.

- In this sesion, we are going to look at another tools that les us do a different sort of memoization: the `useMemo` hook.

- The fundamental idea with the `useMemo` is that is allow sus to "remember" a computed value between renders.

- Generally use this hook for performance optimaizations. It can be used in two seperate but related ways:

1. Can reduce the amount of work that need to be done in a given render.
2. Can reduce the number of times taht a component is re-rendered.

#### Use case 1: Heavy computations

- Building a tool to help users find all of the prime numbers between 0 and `selectedNum`, where `selectedNum` is a user-supplied value. A prime number is a number hat can only be divided by 1 and itself, like 17.

- [Solution, Code Playground](https://codesandbox.io/s/h7j2m9?file=/App.js&utm_medium=sandpack)

```JAVASCRIPT
import React from 'react';

function App() {
  // We hold the user's selected number in state.
  const [selectedNum, setSelectedNum] = React.useState(100);
  
  // We calculate all of the prime numbers between 0 and the
  // user's chosen number, `selectedNum`:
  const allPrimes = [];
  for (let counter = 2; counter < selectedNum; counter++) {
    if (isPrime(counter)) {
      allPrimes.push(counter);
    }
  }
  
  return (
    <>
      <form>
        <label htmlFor="num">Your number:</label>
        <input
          id="num"
          type="number"
          value={selectedNum}
          onChange={(event) => {
            // To prevent computers from exploding,
            // we'll max out at 100k
            const num = Math.min(
              100_000,
              Number(event.target.value)
            );
            
            setSelectedNum(num);
          }}
        />
      </form>
      <p>
        There are {allPrimes.length} prime(s) between 1 and {selectedNum}:
        {' '}
        <span className="prime-list">
          {allPrimes.join(', ')}
        </span>
      </p>
    </>
  );
}

// Helper function that calculates whether a given
// number is prime or not.
function isPrime(n){
  const max = Math.ceil(Math.sqrt(n));
  
  if (n === 2) {
    return true;
  }
  
  for (let counter = 2; counter <= max; counter++) {
    if (n % counter === 0) {
      return false;
    }
  }

  return true;
}

export default App;
```

- In this example, we have a single piece of state, `selectedNum`. Using a for loop, we manually calculate all the prime numbers between 0 and `selectedNum`. the user can cahnge `selectedNum` by editing a controlled number input.

- This code requires a significant amount of computation. If user picks a large `selectedNum`, have to go through tens of thousand of numbers, checking each one is prime.

- We need to do this work atleast once, and again whenever the user picks a new number.

- Another example, suppose we add a digital clock, using the `useTime` hook we created.

- [Code Example, Prime Number and Timer](https://codesandbox.io/s/k4jpss?file=/App.js&utm_medium=sandpack)

- Now, our application now has two pieces of state, `selectedNum` and `time`.
- Once per second, the `time` variable is updated to reflect the current time and that value is used to render a digital clock in the top right corner.

- Here is the issue: whenever either of these state varibales change, the component re-renders adn we re-run all these expsnsive computatoins.
- And because time changes once per second, it means we are constantly re-generating a list of primes, even when the user number has not changed.

![Prime Number](images/image-19.png)

- In JS, we only have one main thred, and we are keeping it super busy by running thsi code over and over, every single second.
- Application will feel sluggish as user tries to do other things, especially on lower end devices.

- **What if we could skip these calculations?** If we already have a list of primes for a given number, why not re-use that value instead of calculating it from scratch every time?

- This is precisely what useMemo allows us to do. Here's what it looks like:

```JAVASCRIPT
const allPrimes = React.useMemo(() => {
  const result = [];

  for (let counter = 2; counter < selectedNum; counter++) {
    if (isPrime(counter)) {
      result.push(counter);
    }
  }

  return result;
}, [selectedNum]);
```

- `useMemo` takes two argumants:

1. A chunk of work to be performed, wrapped in a callback function
2. A lsit of dependencies

During mount, when this component is rendered for the very first time, Rect will invoke this function to run all of this logic, calculating all of the primes. Whatever we return from this funcitno is assinged to the `allPrimes` variable.

For every subsequent render, react ha sa choice to make. Should it:

1. Invoke the function again, to re-calcualte the value, or
2. Re-use the data it already has, from the last ime it did this work.

To answer this, React looks to the supplied list of dependencies. Have any of them changed since the previous render?
If so, react will rerun the supplied funciton, to calcualte a new value. Otherwise, it will skip all that work and reuse the previously-calculated value.

🤔 Similar to other stuff
The structure of `useMemo` is a lot like the structure for `useEffect`. They both take a callback funciton and a dependency array.

- The main difference, is that `useMemo` is used to calculate a value **during render.**
- Effects, meanwhile, invoke the callback function after the render, **to synchronize React state wtih some sort of external system.**

- You might have also noticed: the `useMemo` hook as a similar name to the `React.memo` helper we saw in the previous lesson.

➡️ `React.memo`, memoizes the result of rendering a component, only re-running when the props change.
➡️ `React.useMemo`, memoizes the result of a computation, only re-running when the dependencies change.

- Solution using the `useMemo` hook: [Code Playground](https://codesandbox.io/s/knjmwx?file=/App.js&utm_medium=sandpack)

```JAVASCRIPT
import React from 'react';
import format from 'date-fns/format';

import useTime from './use-time';

function App() {
  const [selectedNum, setSelectedNum] = React.useState(100);
  const time = useTime();
  
  const allPrimes = React.useMemo(() => {
    const result = [];
    
    for (let counter = 2; counter < selectedNum; counter++) {
      if (isPrime(counter)) {
        result.push(counter);
      }
    }
    
    return result;
  }, [selectedNum]);
  
  return (
    <>
      <p className="clock">
        {format(time, 'hh:mm:ss a')}
      </p>
      <form>
        <label htmlFor="num">Your number:</label>
        <input
          id="num"
          type="number"
          value={selectedNum}
          onChange={(event) => {
            // To prevent computers from exploding,
            // we'll max out at 100k
            let num = Math.min(100_000, Number(event.target.value));
            
            setSelectedNum(num);
          }}
        />
      </form>
      <p>
        There are {allPrimes.length} prime(s) between 1 and {selectedNum}:
        {' '}
        <span className="prime-list">
          {allPrimes.join(', ')}
        </span>
      </p>
    </>
  );
}

function isPrime(n){
  const max = Math.ceil(Math.sqrt(n));
  
  if (n === 2) {
    return true;
  }
  
  for (let counter = 2; counter <= max; counter++) {
    if (n % counter === 0) {
      return false;
    }
  }

  return true;
}

export default App;
```

### Use case 2: Preserved references

- We have seen how `useMemo` can help improve performance by caching expensive calculations. This is one way this hook can be used. But not the only way. Another use case.

- In the example below, we have a Boxes component. It displays a set of colorful boxes, to be used for some sort of decorative purpose. It also has a bit of unrelated state, the user's name.

- [Code Playground:](https://codesandbox.io/s/r64v7p?file=/App.js&utm_medium=sandpack)

```JAVASCRIPT
import React from 'react';

import Boxes from './Boxes';

const PureBoxes = React.memo(Boxes);

function App() {
  const [name, setName] = React.useState('');
  const [boxWidth, setBoxWidth] = React.useState(1);
  
  const id = React.useId();
  
  // Try changing some of these values!
  const boxes = [
    { flex: boxWidth, background: 'hsl(345deg 100% 50%)' },
    { flex: 3, background: 'hsl(260deg 100% 80%)' },
    { flex: 1, background: 'hsl(50deg 100% 60%)' },
  ];
  
  return (
    <>
      <PureBoxes boxes={boxes} />
      
      <section>
        <label htmlFor={`${id}-name`}>
          Name:
        </label>
        <input
          id={`${id}-name`}
          type="text"
          value={name}
          onChange={(event) => {
            setName(event.target.value);
          }}
        />
        <label htmlFor={`${id}-box-width`}>
          First box width:
        </label>
        <input
          id={`${id}-box-width`}
          type="range"
          min={1}
          max={5}
          step={0.01}
          value={boxWidth}
          onChange={(event) => {
            setBoxWidth(Number(event.target.value));
          }}
        />
      </section>
    </>
  );
}

export default App;
```

- Our Boxes component has been made pure by `React.memo()`. This means that it should only re-render whenever its props change.
- And yet, wheneve rthe user changes their name, PureBoxes re-renders as well.

![Alt text](images/image-20.png)

- What's going on? Why isn't our React.memo() force field protecting this?

- The PureBoxes compoennt only has 1 prop, boxes, and it appears as though we are giving it hte exact saem data on every render.
- It is always the same thing. We do have a `boxWidth` stae variable that affects the boxes array, but we are not changing it.

- Here's the problem, evertime React re-renders, we are producing a brand new array. **They are equivalent in terms of value, but not in terms of reference.**

- Forget about React for a moment and talk about vanilla JS.

```JAVASCRIPT
function getNumbers() {
    return [1,2,3];
}

const firstResult = getNumbers();
const secondResult = getNumbers();

console.log(firstResult === secondResult);
```

- What do you think the value of the console log is? `true` or `false`?
- In a way, they are both equal, the values are equal. But that isn't what `===` operator is checking.

- Instead, the `===` is checking whether the two experessoins are the same thing.

- Covered in the "Immutability Revisited" lesson. When it comes to objects and arrays, it is not enough for them to look the same.
- They have to be the same. Both variables need to point to the same entity held in the computer's memory.

- Every time we invoke the `getNumbers` function, we create a brand-new array.
- A distict thing held in the computer's meemory.
- If we invoke it multiple times, we will store multiple copies of this array in-memory.

- ℹ️ Note that simple data types - things like `strings`, `numbers`, and `boolean` values - can be compared by value.
- But when it comes to `arrays` and `objects`, they are only compared by reference.

- Check out the [visual guide to references in JS](https://daveceddia.com/javascript-references/).

- Takign this back to React: Our PureBoxes React copoennt is also a JS function. When we render it, we invoke that function.

```JAVASCRIPT
// Every time we render this component, we call this function...
function App() {
  // ...and wind up creating a brand new array...
  const boxes = [
    { flex: boxWidth, background: 'hsl(345deg 100% 50%)' },
    { flex: 3, background: 'hsl(260deg 100% 40%)' },
    { flex: 1, background: 'hsl(50deg 100% 60%)' },
  ];

  // ...which is then passed as a prop to this component!
  return (
    <PureBoxes boxes={boxes} />
  );
}
```

- When the `name` state changes, our `App` component re-renders, which re-runs all of the code. We construct a brand-new `boxes` array and pass it onto our `PureBoxes` component.

- **And PureBoxes re-renders, because we gave ti a brand new array!**

- The structure of the boxes array hasn't changed between renders, but that isn't relevant.
- All React knows, is that the `boxes` prop has received a freshly-created, never-before-seen array.

- To solve this problem, we can use the `useMemo` hook:

```JAVASCRIPT
const boxes = React.useMemo(() => {
    return [
    { flex: boxWidth, background: 'hsl(345deg 100% 50%)' },
    { flex: 3, background: 'hsl(260deg 100% 40%)' },
    { flex: 1, background: 'hsl(50deg 100% 60%)' },
    ];
}, [boxWidth]);
```

- Unlike the example we saw earlier, with the prime numbers, we are not worried about a compuationally expensive calculation here.
- Our only goal si to preserve a reference to a particulat array.

- We list `boxWidth` as a dependency, because we do want the `PureBoxes` component to re-render when the user tweaks the width of the red box.

- Before, does not use `useMemo`, and craetes a brand new array each time everytime a new snapshot is created.

![Alt text](images/image-21.png)

- After, with `useMemo`, we are re-using a previously created `boxes` array:

![Alt text](images/image-22.png)

- 📣 By preserving the same reference accross multiple renders, we allow pure components to function the way we wnat them to, ignoring renders that do not effect the UI.

- [An updated sand box with the useMemo fix.](https://codesandbox.io/s/3jm2gt?file=/App.js&utm_medium=sandpack)

```JAVASCRIPT
import React from 'react';

import Boxes from './Boxes';

const PureBoxes = React.memo(Boxes);

function App() {
  const [name, setName] = React.useState('');
  const [boxWidth, setBoxWidth] = React.useState(1);
  
  const id = React.useId();
  
  const boxes = React.useMemo(() => {
    return [
      { flex: boxWidth, background: 'hsl(345deg 100% 50%)' },
      { flex: 3, background: 'hsl(260deg 100% 40%)' },
      { flex: 1, background: 'hsl(50deg 100% 60%)' },
    ];
  }, [boxWidth]);
  
  return (
    <>
      <PureBoxes boxes={boxes} />
      
      <section>
        <label htmlFor={`${id}-name`}>
          Name:
        </label>
        <input
          id={`${id}-name`}
          type="text"
          value={name}
          onChange={(event) => {
            setName(event.target.value);
          }}
        />
        <label htmlFor={`${id}-box-width`}>
          First box width:
        </label>
        <input
          id={`${id}-box-width`}
          type="range"
          min={1}
          max={5}
          step={0.01}
          value={boxWidth}
          onChange={(event) => {
            setBoxWidth(Number(event.target.value));
          }}
        />
      </section>
    </>
  );
}

export default App;
```

### The useCallback Hook

- How is `useCallback` any different than `useMemo`?

- 🤔 It solves the same "preserved references" problem as `useMemo`, but for functions in stead of arrays and objects.

- Similar to arrays and objects, functions are compared by reference, not by value:

```JAVASCRIPT
const functionOne = function(){ return 5; };
const functionTwo = function(){ return 5; };

console.log(functionOne === functionTwo); //false
```

- This means, that if we define a funciton wthiin oru components, it will be re-generated on every single render, producing an identical but unique funciton each time.

- Example; 👩‍💻 [Code Playground](https://codesandbox.io/s/gkx80v?file=/App.js&utm_medium=sandpack)

```JAVASCRIPT
import React from 'react';

import MegaBoost from './MegaBoost';

function App() {
  const [count, setCount] = React.useState(0);

  function handleMegaBoost() {
    setCount((currentValue) => currentValue + 1234);
  }

  return (
    <>
      Count: {count}
      <button
        onClick={() => {
          setCount(count + 1)
        }}
      >
        Click me!
      </button>
      <MegaBoost handleClick={handleMegaBoost} />
    </>
  );
}

export default App;
```

- The example, a typical counter applicaiton, but with a special "mega Boost" button
- The button increases the count by a large amount, in case you are in a hurry and don't watn to click teh standard button a bunch of times.

- The `MegaBoost` component si a pure component, thanks to `React.memo` (wrapping around the default export inside `MegaBoose.js`).
- This component doesn't receive `count` as a prop, but it re-renders whenever `count` changes!

- 🤔 The problem here is that we are generating a brand new runction on every render.
- If we render 3 times, we will create 3 seperate `handleMegaBoost` functions, breaking through the `React.memo` force field.

- It's the same problem we saw with the `boxes` array in the last lesson, but instead of a twin array being passed as a prop, we have a twin function being passed as a prop.

- Using what we have learned about useMemo, we could solve the prolem like this:

```JAVASCRIPT
const handleMegaBoost = React.useMemo(() => {
    return function() {
        setCount((currentValue) => currentValue + 1234);
    }
}, []);
```

- Instead of returning an array, we are returning a function. This function is then stored in the `handleMegaBoost` variable.
- This works...but a better way to do to this:

```JAVASCRIPT
const handleMegeBoost = React.useCallback(() => {
    setCount((currentValue) => currentValue + 1234);
}, []);
```

- 📣 `useCallback` serves teh same purpose as `useMemo`, but its build specifically for functions.
- We ahnd it  function direclty, and it momoizes that function, threading it between renders.

- Put another way, these two expressoins have the same effect:

```JAVASCRIPT
// This: 
React.useCallback(function helloWorld(){}, []);

// is functionally equivalent to this:
React.useMemo(() => function helloWorld(){}, []);
```

- Basically, useCallback is syntactic sugar, to make our lives a bit easier when trying to memoize callback functions.

#### This stuff is hard 🤔

- It's difficult to get comfortable with `useMemo` and `useCallback`.

- Good news, you can build complex, apps without using either of these hooks.

- These hooks are advanced and inted to optimize applications. But React is pretty fast out of the box.
- If struggling with these momoization lessons, set them aside and come back in a few months after you get more comfortable with React.

### Exercises, Memoization

#### A pure grid

- In the exercise, you find a customizable two dimensional grid. In teh same applicaiton, we are trackign the user's mouse position, and displaying it above the grid.

- The task, is to optimize the grid so that it doesn't have to re-render when the user's mouse position changes.

ACs

- `Grid` should only re-render when either `numRows` or `numCols` changes. Moving the mouse across the viewport should not re-render the Grid component.
- [Code Sandbox](https://codesandbox.io/s/u5yrhu?file=/App.js&utm_medium=sandpack)

```JAVASCRIPT
import React from 'react';

import { range } from './utils';

function Grid({ numRows, numCols }) {
  console.info('Grid render');
  
  return (
    <div className="grid-wrapper">
      {range(numRows).map((rowIndex) => (
        <div key={rowIndex} className="row">
          {range(numCols).map((colIndex) => (
            <div key={colIndex} className="cell" />
          ))}
        </div>
      ))}
    </div>
  );
}

// wrap the export in a memo, so any component that imports the component, get the memoize version
export default React.memo(Grid);
```

#### Shopping Cart

- Revisit the "Shopping Car" exercise back in Module 1
- Update the code so that `CartTable` doesn't re-render unnecessarily.

ACs:

- Editing the postal code should not trigger a re-render in the `CartTable` component.

- Tip: On this one, everytime a new array is generated via the `items.filter()`, it triggers a re-render.
- Add `React.memo()` to the `CardTale` export.

```JAVASCRIPT
// Card table component
import React from 'react';

function CardTable() {
    // component code here
}
export default React.memo(CartTable);
```

- Then, in the shopping cart, add `useMemo()` hook to the `inStockItems` and `outOfStockItems`. This will prevent a re-render everytime user enters something in the postal code field.

```JAVASCRIPT
// Shopping Cart Component
import React from 'react';

import CartTable from './CartTable';

function ShoppingCart({ items }) {
  const [postalCode, setPostalCode] = React.useState('');
  const postalCodeId = React.useId();
 
  // .filter generates a new array, which triggers a re-render
  // wrap in a useMemo hook, and pass in the items array
  // this will only re-render if an update in the items array
  const inStockItems = React.useMemo(() => {
     return items.filter(
        (item) => item.inStock
      );
    }, [items]);

  // do the same thing for the out of stock items
  const outOfStockItems = React.useMemo(() => {
     return items.filter(
        // flip the logic with !
        (item) => !item.inStock
      );
    }, [items]);

  return (
    <>
      <h2>Shopping cart</h2>
      
      <form>
        <label htmlFor={postalCodeId}>
          Enter Postal / ZIP code for shipping estimate:
        </label>
        <input
          id={postalCodeId}
          type="text"
          value={postalCode}
          onChange={(event) => {
            setPostalCode(event.target.value);
          }}
        />
      </form>
      
      <CartTable items={inStockItems} />

      <div className="actions">
        <button>Continue checkout</button>
      </div>

      <h2>Sold out</h2>
      <CartTable items={outOfStockItems} />
    </>
  );
}

export default ShoppingCart;
```

#### Memoized Click Toggle

- In the Custom Hooks exercises, we created a handy `useToggle` hook:

```JAVASCRIPT
function useToggle(initialValue = false) {
  if (typeof initialValue !== 'boolean') {
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
```

- We have updated our digital clock app to use this hook.
- In the [sand box](https://codesandbox.io/s/xrng0i?file=/App.js&utm_medium=sandpack), everything works, but our `ClockToggle` component is rendering on every state change, even once that don't affect it.

- Task, update code so that `ClockToggle` doesn't re-render unnecessarily.

AC's

- The `ClockToggle` component should become a pure component.
- `ClockToggle` should not re-render when the `time` or `showClock` state variables change.

- Tricky exercise, the custom hook `use-toggle.js` was creating the re-render by running a new fuction based on the updated state dependency .
- Solution, was to add a `useCallback` hook, on the function within the `useToggle` component.
- The hard part, is tracking down, where the re-rending is happening based on the state change.

```JAVASCRIPT
// use-toggle.js
import React from 'react';

function useToggle(initialValue = false) {
  if (typeof initialValue !== 'boolean') {
    console.warn('Invalid type for useToggle');
  }

  const [value, setValue] = React.useState(
    initialValue
  );

  // this function is causing the re-render
  // add the useCallback hook here
  const toggleValue = React.useCallback(
    function toggleValue() {
      setValue((currentValue) => !currentValue);
  }, []);


  return [value, toggleValue];
}

export default useToggle;
```

#### Alternatives

- If you focus on the struction of your `App` component, you can get the benefit of easier to understand code, and components that don't collide with one another causing a re-render.
