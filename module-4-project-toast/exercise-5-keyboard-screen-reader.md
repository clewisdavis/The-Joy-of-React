# The Joy of React - Project, Toast Component

- [Course Outline Notes](../course-notes.md)

- Exercise 1: [Wiring up form controls](./exercise-1-wiring-up.md)
- Exercise 2: [Live editable toast preview](./exercise-2-toast-preview.md)
- Exercise 3: [Toast Shelf](./exercise-3-toast-shelf.md)
- Exercise 4: [Context](./exercise-4-context.md)
- Exercise 5: [Keyboard and Screen Reader Support](./exercise-5-keyboard-screen-reader.md)
- Exercise 6: [Extracting custom hooks](./exercise-6-custom-hooks.md)

## Exercise 5: Keyboard and Screen Reader Support

In this exercise, we will improve the experience for two different groups of people:

- Sighted keyboard users
- Users who use a screen reader

### 5.1 Keyboard Users

Let's try something, pretend you do not have a mouse or track pad, using the keyboard alone, create and dismiss a toast message?

The UX is not so great when trying to dismiss. In this exercise, we will wire up the escape key to dismiss the toast.

AC's:

- Hitting the `Escape` key should dismiss all toasts
- You'll want to do this with a `useEffect` hook, but it's up to you do decide which component should do this.
