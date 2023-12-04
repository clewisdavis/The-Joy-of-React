# The Joy of React - Project, Toast Component

- [Course Outline Notes](../course-notes.md)

- Exercise 1: [Wiring up form controls](./exercise-1-wiring-up.md)
- Exercise 2: [Live editable toast preview](./exercise-2-toast-preview.md)
- Exercise 3: [Toast Shelf](./exercise-3-toast-shelf.md)
- Exercise 4: [Context](./exercise-4-context.md)
- Exercise 5: [Keyboard and Screen Reader Support](./exercise-5-keyboard-screen-reader.md)
- Exercise 6: [Extracting custom hooks](./exercise-6-custom-hooks.md)

## Exercise 4: Context

Currently, all of our state is managed by `ToastPlayground`. This will work well for our little app, but will not scale.

In this exercise, we will refactor our app to use the Provider component pattern. It will own all of the state related to the toasts state, and make it available to any child component who require it.

ACs:

1. Create a new component, `ToastProvider`, that will serve as the 'keeper' for all toast-related state.

   - To generate a new component, you can use the 'new-component' script! Try running `npm run new-component ToastProvider` in the terminal

2. Components that require the state should pull it from context with the `useContext` hook, rather than passing through props.

3. As we saw in the 'Provider Components' lessons, we can also share functions that allow consumers to alter the state. Consider making function available that will create a new toast, or dismiss a specific toast.

4. This is a 'refactor' exercise. The UX should not change at all.

- Again, have to review the solution video and go back to your context notes, this was a hard one as-well.

**Exercise Notes:**

- First, create a new provider component, `ToastProvider`, the app is using Parcel js, and has some build in commands for creating components. `npm run new-component ToastProvider`, this will set the component for you. Really nice.

- Remember the basic formula for Context:

- Create the new context with `React.createContext`
- Use the `Provider` component, to wrap around the application. Pass in a bundle of values you need in other parts of the app.

```JAVASCRIPT
import React from 'react';

export const ToastContext = React.createContext();

function ToastProvider({ children }) {
  const [toasts, setToasts] = React.useState([]);

  return (
    <ToastContext.Provider value={{ toasts }}>
      {children}
    </ToastContext.Provider>
  );
}

export default ToastProvider;
```

- Then import an use this provider, around the entire application, so you 📻 broadcast it, to use anywhere in the app.

```JAVASCRIPT
import React from 'react';

import ToastProvider from '../ToastProvider';
import ToastPlayground from '../ToastPlayground';
import Footer from '../Footer';

function App() {
  return (
    <ToastProvider>
      <ToastPlayground />
      <Footer />
    </ToastProvider>
  );
}

export default App;
```

- Pluck the data you need with the `useContext` hook. Now that you are broadcasting the toasts throughout the app. You can use it in your `ToastShelf` component, to pluck out those values.

```JAVASCRIPT
import React from 'react';

import { ToastContext } from '../ToastProvider';
import Toast from '../Toast';
import styles from './ToastShelf.module.css';

function ToastShelf() {
  const { toasts } = React.useContext(ToastContext);

  return (
    <ol className={styles.wrapper}>
      {/* Unchanged */}
    </ol>
  );
}

export default ToastShelf;
```

- Bring in the actions, functions for creating a new toast, and dismiss.
- Full component of the provider:

```JAVASCRIPT
import React from 'react';

// Create the Context
export const ToastContext = React.createContext();

function ToastProvider({ children }) {

   // Update to be an array to hold all the variants, objects, don't forget to generate the key
  const [toasts, setToasts] = React.useState([
    {
      id: crypto.randomUUID(),
      message: 'it works',
      variant: 'error',
    },
    {
      id: crypto.randomUUID(),
      message: 'pulling from provider',
      variant: 'success',
    }
  ]);

  // Function to create the toasts
  function createToast(message, variant) {
    // create a new array, do not mutate the state
    const nextToast = [
      // copy the current toast
      ...toasts,
      {
        id: crypto.randomUUID(),
        message,
        variant,
      }
    ];
    // call the state setter and pass along the new array
    setToasts(nextToast);
  }

  // Create the dismiss
  function dismissToast(id) {
    // create a new array, includes all the items except the one we want to remove
    const nextToast = toasts.filter(toast => {
    //   // go through all the toast, and find the one trying to dismiss
    //   // 🤔 keep the toast, if the id is NOT equal to the one we are dismissing
      return toast.id !== id
    })

    // // call state setter function passing in the new array
    setToasts(nextToast);
  }

  return (
    // Set up the provider and broadcast our state
    <ToastContext.Provider 
      value={{ 
          toasts,
          createToast,
          dismissToast
      }}
    >
      { children }
    </ToastContext.Provider>
  )
}

export default ToastProvider;

```
