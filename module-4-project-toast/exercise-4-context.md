# The Joy of React - Project, Toast Component

- [Course Outline Notes](course-notes.md)

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

- Create a new component, `ToastProvider`, that will serve as the 'keeper' for all toast-related state.
  - To generate a new component, you can use the 'new-component' script! Try running `npm run new-component ToastProvider` in the terminal
- Components that require the state should pull it from context with the `useContext` hook, rather than passing through props.
- As we saw in the 'Provider Components' lessons, we can also share functions hat allow consumers to alter the state. Consider making function available that will create a new toast, or dismiss a specific toast.
- This is a 'refactor' exercise. The UX should not change at all.
