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

- For now, you can impor teh `Toast` component in the `ToastPlayground` and render it between the header and the controls:
