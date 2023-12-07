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

- Notes:

- Which component should hold this functionality? Put it in the provider, makes the most sense, where the data lives, and where you want to manage everything related to the toasts.

- Create your `useEffect` hook, and set up the listeners for the keyboard function. And the cleanup.
- Hitting 'Escape' will remove ALL the toasts.

```JAVASCRIPT
// inside ToastProvider.js

// Create the Context
export const ToastContext = React.createContext();

function ToastProvider({ children }) {

  // Adding a useEffect to listen for the 'Escape' key to remove ALL the toasts from state
  React.useEffect(() => {
        console.log('toast shelf mounted');

        // create the keyDown function
        function handleKeyDown(event) {
        if (event.code === 'Escape') {
            console.log('keydown');
            // reset all the toast, calling the state setter function and passing a new empty array
            setToasts([]);
        }
        }

        // create a listener on mount
        window.addEventListener('keydown', handleKeyDown);

        // cleanup
        return () => {
        window.removeEventListener('keydown', handleKeyDown);
        };
    }, []);

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

### 5.2 Screen Reader Users

A screen reader is a piece of software that narrates the page. Primarily for low vision or folks that are blind. But can be used for other cognitive reasons.

- Imagine we reach out to an accessibility specialist, and they do us a favor of converting our HTML to be screen reader friendly. We need to add some markup to the HTML.

```HTML
<ol
  class="ToastShelf_wrapper"
+ role="region"
+ aria-live="polite"
+ aria-label="Notification"
>
  <li class="ToastShelf_toastWrapper">
    <div class="Toast_toast Toast_error">
      <div class="Toast_iconContainer">
        <!-- Variant SVG icon -->
      </div>
      <p class="Toast_content">
+       <div class="VisuallyHidden_wrapper">
+         error -
+       </div>
        Something went wrong! Please contact customer support
      </p>
      <button
        class="Toast_closeButton"
+       aria-label="Dismiss message"
+       aria-live="off"
      >
        <!-- Close SVG icon -->
-       <div class="VisuallyHidden_wrapper">
-         Dismiss message
-       </div>
      </button>
    </div>
  </li>
</ol>
```

ACs:

- The `<ol>` should have specified role / aria tags
- The toasts content should be prefixed with the variant, using the `VisuallyHidden` component.
  - NOTE: The diff above shows an error toast, but the prefix should be dynamic, based on the variant.
- The 'Dismiss message' content in the close button should be moved to an `aria-label`. `aria-live` should also be set to 'off'.
