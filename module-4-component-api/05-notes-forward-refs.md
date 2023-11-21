# The Joy of React - Module 4 - Component API Design

- [Course Outline Notes](../course-notes.md)

## Component API Design

### Forwarding Refs

- `ref` is a reserved work in React, so you cannot use it as a custom prop on a React component.
- In your `App` component, you are consuming the `Slider` component. Thinking of it as a super charged input. Giving it a bunch of props and you want to capture the input via a `ref`
- This doesn't work, because we are not allowed to capture `references` of functional components, `<Slider />`.

```JAVASCRIPT
// App.js
import React from 'react';

import Slider from './Slider';

function App() {
  const [volume, setVolume] = React.useState(50);
  
  // Create a React ref:
  const sliderRef = React.useRef();
  
  React.useEffect(() => {
    // Focus the slider on mount:
    sliderRef.current.focus();
  }, []);
  
  return (
    <main>
      <Slider
        // Capture a reference to the slider:
        ref={sliderRef}
        label="Volume"
        min={0}
        max={100}
        value={volume}
        onChange={(event) => {
          setVolume(event.target.value);
        }}
      />
    </main>
  );
}

export default App;
```

- To solve this, we can capture that reference, on the actual component.
- If someone tries to use a `ref` on the component, we can do that, my adding the `React.forwardRef` to the export of the component, `Slider`.
- Also adding `ref` as an argument.
- And `ref={ref}` to your element.

```JAVASCRIPT
// Slider.js
import React from 'react';

import styles from './Slider.module.css';

function Slider({ label, ...delegated }, 
  // add the ref
  ref) {
  const id = React.useId();
  
  return (
    <div className={styles.wrapper}>
      <label
        htmlFor={id}
        className={styles.label}
      >
        {label}
      </label>
      <input
        // add ref to your element
        ref={ref}
        {...delegated}
        type="range"
        id={id}
        className={styles.slider}
      />
    </div>
  );
}

// Add the forwardRef helper
export default React.forwardRef(Slider);
```

- Then in `App.js`, you can just pass along the `ref`.

```JAVASCRIPT
import React from 'react';

import Slider from './Slider';

function App() {
  const [volume, setVolume] = React.useState(50);
  
  // Create a React ref:
  const sliderRef = React.useRef();
  
  React.useEffect(() => {
    // Focus the slider on mount:
    sliderRef.current.focus();
  }, []);
  
  return (
    <main>
      <Slider
        // Capture a reference to the slider:
        ref={sliderRef}
        label="Volume"
        min={0}
        max={100}
        value={volume}
        onChange={(event) => {
          setVolume(event.target.value);
        }}
      />
    </main>
  );
}

export default App;
```

- 🤔 This is known as a higher-order component in React. It takes a component as input, and produces a new component as output.

- Note, if you want to use `React.memo` and `React.forwardRef` on the same component?

```JAVASCRIPT
export default React.memo(React.forwardRef(Slider));
```

#### Exercises, Supercharged Button

Get some practice forwarding refs along.

- In the [sandbox](https://codesandbox.io/s/755kcr?file=/App.js&utm_medium=sandpack), have a `<Button>` component, and we want to set i tup to be a 'supercharged' button. This means we should be able to capture a reference to it, as well as forward all props along to it.

ACs:

- When hovering or focusing the button in the DOM, it should log "Captured ref: HTMLElement" in the console.
- There should be no console warning.

- My solution:

```JAVASCRIPT
// Button.js
import React from 'react';

import styles from './Button.module.css';

// Add ref as a parameter to forward along
// And add the ...delegated prop
function Button({ children, ...delegated }, ref) {
  return (
    <button 
      className={styles.btn} 
      // Put ref on the element
      ref={ref}
      // Spread any props onto the button
      {...delegated}
    >
      {children}
    </button>
  );
}

// Add the React.forwardRef
export default React.forwardRef(Button);
```

- App.js

```JAVASCRIPT
// App.js
import React from 'react';

import Button from './Button';

function App() {
  // Capture the element
  const buttonRef = React.useRef();
  
  // Console log the element on onFocus
  function logRef() {
    console.log('Captured ref:', buttonRef.current);
  }
  
  return (
    <Button
      // add ref as prop
      ref={buttonRef}
      onMouseEnter={logRef}
      onFocus={logRef}
    >
      Hover or Focus Me
    </Button>
  );
}

export default App;
```

#### Exercise, SquareSlider forwarding

One of the great things about Rect is the ability to 'compose' components. For example, we can build a `SquareSlider` component that builds on the lower-level `Slider` component, locking in specific values.

In the sandbox below, we are trying to capture a ref on a `<SquareSlider>` component, but it isn't working. Your mission is to fix it.

ACs

- The `sliderRef` ref should hold a reference to the `<input type="range">`.
- You can verify this in teh console. It should show that `sliderRef` holds an `HTMLElement`.

- Code Sandbox - [Square Slider Forwarding](https://codesandbox.io/s/mfr63n?file=/App.js&utm_medium=sandpack)

- In this one, you have to pass down the `ref` to the lower level component. `App` > `SquareSlider` > `Slider`. Using the `React.forwardRef` on the two lower level components.

```JAVASCRIPT
// App.js
import React from 'react';

import SquareSlider from './SquareSlider';

function App() {
  // Define the ref
  const sliderRef = React.useRef();
  
  React.useEffect(() => {
    // Log the value on mount
    console.log(sliderRef.current);
  }, []);
  
  return (
    <main>
      <SquareSlider
        // Define the ref prop
        ref={sliderRef}
        label="Intensity"
        min={0}
        max={10}
      />
    </main>
  );
}

export default App;
```

- `SquareSlider` Component

```JAVASCRIPT
// SquareSlider.js
import React from 'react';

import Slider from './Slider';
import styles from './SquareSlider.module.css';

// add ref prop
function SquareSlider(props, ref) {
  return (
    <Slider 
      // pass down to lower level
      ref={ref}
      {...props}
      className={styles.squareSlider} 
    />
  );
}

// wrap in forwardRef, to pass it down to lower level
export default React.forwardRef(SquareSlider);
```

- And low level `Slider` component

```JAVASCRIPT
// Slider.js
import React from 'react';

import styles from './Slider.module.css';

// add the ref as a parameter
function Slider({ label, className = '', ...delegated }, ref) {
  const id = React.useId();
  
  return (
    <div className={styles.wrapper}>
      <label
        htmlFor={id}
        className={styles.label}
      >
        {label}
      </label>
      <input
        {...delegated}
        // define the element to use ref
        ref={ref}
        type="range"
        id={id}
        className={`${styles.slider} ${className}`}
      />
    </div>
  );
}

// Wrap in an forwardRef
export default React.forwardRef(Slider);
```
