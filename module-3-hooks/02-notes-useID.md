# The Joy of React - Module 3 - Course Notes

- [Course Outline Notes](../course-notes.md)

## React Hooks

### The useId Hook

- Reuse, is the main benefit of using react.
- Try to write, any component re-usable. But is difficult for some parts of the web.
- React has a new tool, to give a unique identifier to an instance of a component.

```JAVASCRIPT
const id = React.useId();
```

- Gives a unique ID, to each instance of a given component. For example; if you have a form, and you want to give a unique id for each text input, label.

- If you console log the `id`, you get a unique id that corresponds with each instance of the component.
- React knows, how many instances of a given component exist, and it stores that info on the instance.
- The useId Hook, let's us hook into that internal data and access that id.

- Now you can use that unique id, on a form, and create a unique string.

```JAVASCRIPT
import React from 'react';

function LoginForm() {
  const [username, setUsername] = React.useState('');
  const [password, setPassword] = React.useState('');

  // Pluck this instance's unique ID from React
  const id = React.useId();

  // Create element IDs using this unique ID
  const usernameId = `${id}-username`;
  const passwordId = `${id}-password`;
  
  return (
    <form className="login-form">
      <div>
        {/* Apply these IDs to the label and input */}
        <label htmlFor={usernameId}>
          Username:
        </label>
        <input
          type="text"
          id={usernameId}
          value={username}
          onChange={event => {
            setUsername(event.target.value);
          }}
        />
      </div>
      <div>
        <label htmlFor={passwordId}>
          Password:
        </label>
        <input
          type="password"
          id={passwordId}
          value={password}
          onChange={event => {
            setPassword(event.target.value);
          }}
        />
      </div>
      <button>
        Submit
      </button>
    </form>
  );
}

export default LoginForm;
```

- How about using an alternative to create a unique id, like `Math.random()`
- But when you enter information into the field, you trigger a state change, and a new number will be created with the `Math.random()`. Notice the `React.useId()` is stable across renders.
- Because we are hooking into an instance, and the instance doesn't change between renders. But the `Math.random()` does change with each re-render of the component.
- Whenever we re-render the component, we call the function again, so `Math.random()` will generate a new number.
- Special thing with Hooks, we are able to pass things along between renders.

### Practice - Toggle Component

- A `Toggle` component, finish it up by adding a unique ID to the button, and connecting it ot the label. You should be able to trigger the toggle by clicking the 'Dark Mode' text.

- Add the `useId()` method hook. `const id = React.useId();`
- On the switch element label, add the `htmlFor` attribute, `htmlFor={id}`
- Then add the associated `id={id}` to the `button`

```JAVASCRIPT
function Toggle({
  label,
  checked,
  handleToggle,
  backdropColor = 'white',
  size = 16,
}) {
  const id = React.useId();

  const padding = size * 0.1;
  const width = size * 2 + padding * 2;

  const wrapperStyle = {
    width,
    padding,
    '--radius': size * 0.25 + 'px',
    '--backdrop-color': backdropColor,
  };

  const ballStyle = {
    width: size,
    height: size,
    transform: checked ? `translateX(100%)` : `translateX(0%)`,
  };

  return (
    <div className={styles.wrapper}>
      <label htmlFor={id}>
        {label}
      </label>
      <button
        id={id}
        className={styles.toggle}
        type="button"
        aria-pressed={checked}
        style={wrapperStyle}
        onClick={() => {
          handleToggle(!checked)
        }}
      >
        <span className={styles.ball} style={ballStyle} />
      </button>
    </div>
  );
};
```
