# Module 2 - Working with State

- [Course Outline Notes](course-notes.md)

## Prop Vs. State

- When learning React it is normal for the concepts of "props" and "state" to be a bit confusing and intermingled.
- What is the difference between them exactly? When do you use props and when to use state?

### Props

- "Props is short for "properties". At the micro level, the are like the attributes we place on HTML elements, like `class` or `href`:

```JAVASCRIPT
  <a
    class="nav-link"
    href="/category/stuff"
  >
```

- For example, the `Button` component below takes a "variant" prop. This prop will be used internally to control styling, like how the `class` attribute works in HTML.

```JAVASCRIPT
    import Button from './Button';

    function App() {
      return (
        <div className="box">
          <p>
            Are you sure you want to continue?
          </p>
          <div className="actions">
            <Button variant="secondary">
              Cancel
            </Button>
            <Button variant="primary">
              Confirm
            </Button>
          </div>
        </div>
      );
    }

    export default App;
```

- Props allow us to customize behavior of a given component, so the exact same component can do different things in different scenarios.

- 🚀 **Props are the inputs to our components, like arguments passed to a function.**
  
### State

- In the example above, our application is static. every time we run this code, we get the same results.
- But what if we wanted stuff to change over time? **That's where state comes in.**

- Let's tweak our example:

```JAVASCRIPT
    import React from 'react';

    import Button from './Button';

    function App() {
      const [hasAgreed, setHasAgreed] = React.useState(false);
      
      return (
        <div className="box">
          <p>
            Are you sure you want to continue?
          </p>
          <label htmlFor="confirm-checkbox">
            <span className="required">*</span>
            <input
              id="confirm-checkbox"
              type="checkbox"
              value={hasAgreed}
              onChange={() => setHasAgreed(!hasAgreed)}
            />
            <span>
              I agree with <a href="/terms">the terms</a>.
            </span>
          </label>
          <div className="actions">
            <Button
              variant="secondary"
              isEnabled={true}
            >
              Cancel
            </Button>
            <Button
              variant="primary"
              isEnabled={hasAgreed}
            >
              Confirm
            </Button>
          </div>
        </div>
      );
    }

    export default App;
```

- We have a new bit of state, `hasAgreed`, and we need to use that data to power our "confirm" button.
- This reveals an important truth about props: **they are tunnels that allow data to flow through our application.**

- Let's dig in, video
- When you create a state variable, it is only available within the component. It's not global or available to other components.
- Two ways to think about it:
  - Props are used to control the configuration of a particular instance of a component.
  - Props are also, the tracks we use to funnel data from one place to another. **The way, parent components communicate to children components.** They are the tunnels that connect our different components.

- 🤔 The data that we can use in our props, can be a string, boolean, or we can use react state.
- We use a prop, to pass a bit of state to one place to another.

- Props are basically a subway network, a system of tunnels we can use to funnel data around.
