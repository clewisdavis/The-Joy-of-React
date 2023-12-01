# The Joy of React - Project, Toast Component

- [Course Outline Notes](course-notes.md)

- Exercise 1: [Wiring up form controls](./exercise-1-wiring-up.md)
- Exercise 2: [Live editable toast preview](./exercise-2-toast-preview.md)
- Exercise 3: [Toast Shelf](./exercise-3-toast-shelf.md)
- Exercise 4: [Context](./exercise-4-context.md)
- Exercise 5: [Keyboard and Screen Reader Support](./exercise-5-keyboard-screen-reader.md)
- Exercise 6: [Extracting custom hooks](./exercise-6-custom-hooks.md)

## Exercise 3: Toast Shelf

One of hte core defining characteristics of a toast notification is they stack.

![Stack Toast](images/image-1.png)

- In this exercise restructure things so that our `ToastsPlayground` allow us to create multiple toasts.
- For help, you will find a `ToastShelf` component in this project. It will automatically apply the styles and animations.

- You will need to replace the `Toast` live demo with this new `ToastShelf` component, inside the `ToastPlayground`:

```JAVASCRIPT
<header>
  <img alt="Cute toast mascot" src="/toast.png" />
  <h1>Toast Playground</h1>
</header>

// <Toast />
<ToastShelf />

<div className={styles.controlsWrapper}>
  <div className={styles.row}>
```

- By the end of this exercise, it should look like this:

![End result](images/image-2.png)

- This is a very tricky exercise. Use the hints to help get started. And refer back to these lessons.
  - The onClick Parable
  - Dynamic Key Generation

ACs:

1. Instead of live-editing a single Toast instance, the playground should be used to push new toast messages onto a stack, rendered inside `ToastShelf` and shown in the corner of the page.
2. When 'Pop Toast' is clicked, the message/variant form controls should be reset to their default state, `message` should be an empty string, `variant` should be 'notice'.
3. Clicking the 'x' button inside the toast should remove that specific toast (but leave the rest untouched).
4. A proper `<form>` tag should be used int he `ToastPlayground`. The toast should be created when submitting the form.
5. There should be no key warnings in the console. Keys should be unique and should not sure the index.

Solution Notes:

- Add a new state structure, the new state needs to be an array to hold all the toasts, in the `<ToastPlayground>` component.

```JAVASCRIPT
  // Update to be an array to hold all the variants, objects, don't forget to generate the key
  const [toasts, setToasts] = React.useState([
    {
      id: crypto.randomUUID(),
      message: 'Oh No',
      variant: 'error',
    },
    {
      id: crypto.randomUUID(),
      message: 'Logged In',
      variant: 'success',
    }
  ]);
```

- Displaying the toasts in the provided `<ToastShelf>` component.
- Add the new component and pass in `toast` state as a prop.

```JAVASCRIPT
    <ToastShelf
        toasts={toasts}
    />
```

- Inside the provided `ToastShelf` component, add the new `toast` prop, and you will have to map over the array from within the component.

```JAVASCRIPT
function ToastShelf({ toasts }) {
  return (
    <ol className={styles.wrapper}>
      {toasts.map(toast => (
        <li className={styles.toastWrapper}>
          <Toast variant={toast.variant}>
            {toast.message}
          </Toast>
        </li>
      ))}
    </ol>
  );
}
```

- Wiring up the form to push new toasts, create a new function to push the new toasts.
- The function will do the following below.

```JAVASCRIPT
  function handleCreateToast(event) {
    // prevent the default for behavior
    event.preventDefault();

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

    // clear out the state in the form
    setMessage('');
    setVariant(VARIANT_OPTIONS[0]);
  }
```

- And update the JSX, to be in a form format, and use an `onSubmit` handler.

```JAVASCRIPT
    <form onSubmit={handleCreateToast} className={styles.controlsWrapper}>
       // form here
    </form>
```

- We also have to dismiss the toast, with the 'x'
- Create a `handleDismiss` function.

```JAVASCRIPT
  function handleDismiss(id) {
    // not allowed to mutate in React
    // create a new array, includes all the items except the one we want to remove
    const nextToast = toasts.filter(toast => {
      // go through all the toast, and find the one trying to dismiss
      // 🤔 keep the toast, if the id is NOT equal to the one we are dismissing
      return toast.id !== id
    })

    // call state setter function passing in the new array
    setToasts(nextToast);
  }
```

- And pass it into the `<ToastShelf>` component.

```JAVASCRIPT
  <ToastShelf
    handleDismiss={handleDismiss}
    toasts={toasts}
  />
```

- Then, within the `ToastShelf` component, pass it down to the actual `Toast`, where it will be used on an `onClick` event.

```JAVASCRIPT
    <Toast 
      id={toast.id}
      variant={toast.variant}
      handleDismiss={handleDismiss}
    >
      {toast.message}
    </Toast>
```

- Within the `Toast`, add it to the 'x' button
- 🤔 Pass in the `id` so it knows which one to filter out

```JAVASCRIPT
      <button 
        className={styles.closeButton}
        onClick={() => handleDismiss(id)}
      >
        <X size={24} />
        <VisuallyHidden>Dismiss message</VisuallyHidden>
      </button>
```

- 🤔 This was a really hard exercise.
