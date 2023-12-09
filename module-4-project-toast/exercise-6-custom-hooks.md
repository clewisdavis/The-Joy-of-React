# The Joy of React - Project, Toast Component

- [Course Outline Notes](../course-notes.md)

- Exercise 1: [Wiring up form controls](./exercise-1-wiring-up.md)
- Exercise 2: [Live editable toast preview](./exercise-2-toast-preview.md)
- Exercise 3: [Toast Shelf](./exercise-3-toast-shelf.md)
- Exercise 4: [Context](./exercise-4-context.md)
- Exercise 5: [Keyboard and Screen Reader Support](./exercise-5-keyboard-screen-reader.md)
- Exercise 6: [Extracting custom hooks](./exercise-6-custom-hooks.md)

## Exercise 6: Extracting custom hooks

In the previous example, we added a `escape` keyboard shortcut, to dismiss all toasts in a single keystroke. This is a very common pattern, and it requires a surprising amount of boilerplate in React.

- Let's build a custom reusable hook that makes it easy to reuse this boilerplate to solve future problems.
- Lot's of ways to solve, you can create a custom hook called `useEscapeKey`?

```JAVASCRIPT
useEscapeKey(() => {
  // Code to dismiss all toasts
});
```

ACs:

- We want to create a new generic hook that makes it easy to listen for `keydown` events in React. It is up to you to come up with the best 'consumer experience'.
- Because this is a generic hook, it shouldn't be stored with the `ToastProvider` component. Create a new `/src/hooks` directory, and place your new hook in there.
- The `ToastProvider` component should use this new hook.
- Make sure no ESLint warnings. Little squigglies.

### Solution Notes

- Generic hook, pass in the `key` you want to use to initiate.

```JAVASCRIPT
import React from 'react';

// make this universal, passing in a key parameter for the keydown, Escape, LeftArrow, RightArrow, Space, etc. 
function useKeydown(key, callback) {
  React.useEffect(() => {
    function handleKeyDown(event) {
      if (event.code === key) {
        callback(event);
      }
    }

    window.addEventListener('keydown', handleKeyDown);

    return () => {
      window.removeEventListener('keydown', handleKeyDown);
    };
  }, [key, callback]);
}

export default useKeydown;
```

- Import the new custom hook, and use it.

```JAVASCRIPT
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


    // use memoization hook
    const handleEscape = React.useCallback(() => {
      setToasts([]);
    }, []);
    
    // Now, you can pass the keydown you want, arrow, escape etc. 
    useKeydown('Escape', handleEscape);
    // useKeydown('LeftArrow', differentCallback);
    // useKeydown('RightArrow', differentCallback);

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
