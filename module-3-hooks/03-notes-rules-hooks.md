# The Joy of React - Module 3 - Course Notes

- [Course Outline Notes](../course-notes.md)

## React Hooks

### Rules of Hooks

- Hooks are special functions that allow us to "hook" into Rect internals.
- `useState` allows us to hook into a component instance's state, for example, while `useId` allows us to create and store a unique identifier on the component instance.
- What happens if we try to call these functions outside of a react context?
  - You get a warning, stating Hooks can only be called inside a body of a function component. This could happen for one of the following reasons.
  - 1. You might have mismatching versions of React and teh renderer, such as React DOM
  - 2. You might be breaking the Rules of Hooks
  - You might have more than one copy of React in the same app.

- First understand that hooks are plain old JS functions.
- When we call these functions, they hook into React internals.
- React expects hook functions to be used in very specific ways, and if we violate those expectations. Base things can happen.

- 📣 There are two Rules of Hooks that we should learn.
- 1. Hooks have to be called within the scope of a React app. We can't call them outside of our React components.
- 2. We have to call our hooks at the top level of the component. **This rule trips most people up.**

- Cannot call a hook, if it's in a condition statement, or anything that makes it not at the top level of the component. Order matters.
- A nice way to look at it, if the hook is indented, will not work.

```JAVASCRIPT
// will error
function TextInput({ id, label, type }) {
  let appliedId = id;
  if (typeof appliedId === 'undefined') {
    appliedId = ReactuseId();
  }

  return (
    // component markup
  )
}
```

- What if you wanted to use a condition? How would you write that?

```JAVASCRIPT
import React from 'react';

function TextInput({ id, label, type }) {
  // Here's the original code, violating the rule:
  //
  //   let appliedId = id;
  //   if (typeof appliedId === 'undefined') {
  //     appliedId = React.useId();
  //   }
  //
  // ...and here's the fixed code:
  const generatedId = React.useId();
  const appliedId = id || generatedId;

  return (
    <div className="text-input">
      <label htmlFor={appliedId}>
        {label}
      </label>
      <input
        id={appliedId}
        type={type}
      />
    </div>
  );
}

export default TextInput;
```

#### Hooks, exercises

- Fix the violation
- We have a search UI with a conditionally rendered text input. Clicking the icon slides a text input into place.
- The code is a violation of Rules of Hooks! Your mission is to update the code to comply with the rules.

- ACs
  - No lint warnings should be shown.
  - Clicking the search button should reveal a search input.

- Two possible solutions, first is to make sure the hook is at the correct level on the component.

```JAVASCRIPT
// Change this code:
let searchId;
if (showSearchField) {
  searchId = React.useId();
}

// ...to this code:
const searchId = React.useId();
```

- Or you can create a new create a new component and conditionally display the search input.

```JAVASCRIPT
import React from 'react';
import { X, Search } from 'react-feather';

import VisuallyHidden from './VisuallyHidden';

function App() {
  const [
    showSearchField,
    setShowSearchField,
  ] = React.useState(false);

  function handleToggleSearch(event) {
    event.preventDefault();
    setShowSearchField(!showSearchField);
  }

  return (
    <>
      <form>
        {showSearchField && <SearchField />}
        <button
          className="search-toggle-button"
          onClick={handleToggleSearch}
        >
          {showSearchField ? <X /> : <Search />}
          <VisuallyHidden>
            Toggle search field
          </VisuallyHidden>
        </button>
      </form>
    </>
  );
}

// create a new component, and use logical && to show it above
function SearchField() {
  const searchId = React.useId();

  return (
    <div className="search-field-wrapper">
      <label htmlFor={searchId}>
        <VisuallyHidden>Search</VisuallyHidden>
      </label>
      <input
        id={searchId}
        className="search-field"
      />
    </div>
  );
}

export default App;
```

#### Exercise, Fix another violation

Fix another violation with a different scenario

- In this exercise, our LoginForm component can either render:
- A form including email/password fields, if the user isn't logged in.
- A paragraph, if the user is logged in

You can toggle between these states by submitting the form.

- ACs
- Update the code below so that it doesn't violate the Rules of Hooks.

- Solution 1. Moving the early return, grouping the hooks together before you return any conditions.

```JAVASCRIPT
function LoginForm({ isLoggedIn, handleLogin }) {
  const id = React.useId();
  const [email, setEmail] = React.useState('');
  const [password, setPassword] = React.useState('');

  if (isLoggedIn) {
    return (
      <p>You're already logged in!</p>
    );
  }

  return (
    /* Same stuff, omitted for brevity */
  );
}
```

- Solution 2. Removing the early return entirely. Move the `isLoggedIn` to the parent component, `App`
- Use a ternary condition to display the logged in status.

```JAVASCRIPT
function App() {
  const [isLoggedIn, setIsLoggedIn] = React.useState(false);

  function handleLogin(event) {
    // NOTE: In a real application, we'd perform a
    // network request here, to validate the login.
    // We'll see how to do this later in this module.
    event.preventDefault();
    setIsLoggedIn(true);
  }

  return (
    <>
      {isLoggedIn ? (
        <>
          <p>You're already logged in!</p>
          <button
            onClick={(event) => {
              setIsLoggedIn(false);
            }}
          >
            Log Out
          </button>
        </>
      ) : (
        <LoginForm handleLogin={handleLogin} />
      )}
    </>
  );
}
```
