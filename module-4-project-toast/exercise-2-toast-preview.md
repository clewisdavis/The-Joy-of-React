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

- The toast component should show the message entered in the textarea, essentially acting as a 'live preview.
- The toast styling should be affected by the 'variant' selected:
  - The colors can be set by specifying the appropriate class on the top level `<div>`. By default, it is set to `styles.notice` but you will want to dynamically select the class based on the variant, e.g. for success toast, you want to apply `styles.success`.
  - The icon can be selected from the `ICONS_BY_VARIANT` object. Feel free to re-organize things however you wish.
- The toast should be hidden by default, but can be shown by clicking the 'Pop Toast' button.
- The toast can be hidden by clicking the 'x' button within the toast.
