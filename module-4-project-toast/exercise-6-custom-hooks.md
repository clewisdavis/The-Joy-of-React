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
