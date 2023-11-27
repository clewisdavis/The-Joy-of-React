# The Joy of React - Project, Toast Component

- [Course Outline Notes](course-notes.md)

- Exercise 1: [Wiring up form controls](./exercise-1-wiring-up.md)
- Exercise 2: [Live editable toast preview](./exercise-2-toast-preview.md)
- Exercise 3: [Toast Shelf](./exercise-3-toast-shelf.md)
- Exercise 4: [Context](./exercise-4-context.md)
- Exercise 5: [Keyboard and Screen Reader Support](./exercise-5-keyboard-screen-reader.md)
- Exercise 6: [Extracting custom hooks](./exercise-6-custom-hooks.md)

## Exercise 2: Live editable toast preview

Inside the `src/components`, you wil find a `Toast` component. This component includes the basic DOM structure you will need, but it's static and does not accept any props.

In this exercise, the mission is to render the `Toast` component within the `ToastPlayground` and allow the playground to customize the Toast using the state we set up in the previous exercise. We should also figure out a 'dismissal' mechanism, so the close button functions.

- For now, you can import `Toast` component in the `ToastPlayground` and render it between the header and the controls:

```JAVASCRIPT
<header>
  <img alt="Cute toast mascot" src="/toast.png" />
  <h1>Toast Playground</h1>
</header>

{/* Place a <Toast /> here! */}

<div className={styles.controlsWrapper}>
  <div className={styles.row}>
```

- Up to you to com eup with teh best possible 'Prop API' for this component.
- If you get stuck, you can reference the exercises:
  - Styling in React
  - Slots, the stretch goal

ACs:

1. The toast component should show the message entered in the textarea, essentially acting as a 'live preview.
2. The toast styling should be affected by the 'variant' selected:

   - The colors can be set by specifying the appropriate class on the top level `<div>`. By default, it is set to `styles.notice` but you will want to dynamically select the class based on the variant, e.g. for success toast, you want to apply `styles.success`.
   - The icon can be selected from the `ICONS_BY_VARIANT` object. Feel free to re-organize things however you wish.

3. The toast should be hidden by default, but can be shown by clicking the 'Pop Toast' button.
4. The toast can be hidden by clicking the 'x' button within the toast.

**Solution Notes:**

- Create the component, prop API, as if you were the consumer

```JAVASCRIPT
<Toast
    content={message}
    variant={variants}
    handleDismiss={handleDismiss}
/>
```

- Or an alternative to this, would be to use `{children}`

```JAVASCRIPT
<Toast
    variant={variants}
    handleDismiss={handleDismiss}
>
{message}
</Toast>
```

- Add state to manage the `message`, `variant` and boolean value to conditionally render `isRendered`.

```JAVASCRIPT
  // state for 'textarea'
  const [message, setMessage] = React.useState('');
  console.log(message);

  // state for 'radio'
  const [variants, setVariants] = React.useState('notice');
  console.log(variants)

  // pop the toast
  const [isRendered, setIsRendered] = React.useState(false);
  console.log(isRendered);
```

- Apply the CSS classes based on the `variant` prop in the `<Toast>` component.
- Concatenate the classes together, using the square bracket notation, `[]`. Then apply the variable to the element via `className`.

```JAVASCRIPT
  // Concatenate multiple classes into one
  const variantClasses = `${styles.toast} ${styles[variant]}`;

  return (
     <div className={variantClasses}>
       ...
    </div>
  )
```

- The Icon, create a new `Icon` component in the `<Toast>` component. And use provided object `ICONS_BY_VARIANT` with the square bracket notation `[]` and the `variant` prop to select the correct name for the icon.

```JAVASCRIPT
import {
  AlertOctagon,
  AlertTriangle,
  CheckCircle,
  Info,
  X,
} from 'react-feather';

const ICONS_BY_VARIANT = {
  notice: Info,
  warning: AlertTriangle,
  success: CheckCircle,
  error: AlertOctagon,
};

function Toast({content, variant, handleDismiss}) {
    // Assign the icon, using the [] notation
    const Icon = ICONS_BY_VARIANT[variant];

    return (
        <Icon size={24} />
    )
}
```

- Conditionally render the toast 📣 with `&&` using the boolean value of state.

```JAVASCRIPT
      {/* Conditionally render based on the state of isRendered */}
      {isRendered && <Toast
        content={message}
        variant={variants}
        handleDismiss={handleDismiss}
      />}
```

- Add an `onClick` handler to the button to toggle the state to true, and display the toast.

```JAVASCRIPT
    <Button
      onClick={event => {
        setIsRendered(true);
      }}
    >Pop Toast!</Button>
```

- Create a `handleDismiss` function inside the `<ToastPlayground>`, and pass that down as a prop to the toast, for the close button.

```JAVASCRIPT
  // create the handle dismiss function
  function handleDismiss() {
    setIsRendered(false);
  }
```

- On the `<Toast>`, add an `onClick` handler to the close button, to call `handleDismiss` to close the toast.

```JAVASCRIPT
      <button 
        className={styles.closeButton}
        onClick={handleDismiss}
      >
        <X size={24} />
        <VisuallyHidden>Dismiss message</VisuallyHidden>
      </button>
```
