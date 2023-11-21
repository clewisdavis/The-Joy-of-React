# The Joy of React - Module 4 - Component API Design

- [Course Outline Notes](../course-notes.md)

## Module 4

### Component API Design

#### Modals

Just about every modern we app needs a good `<Modal>` component.

A modal is an in-window popup that displays a message. Sometimes, it has buttons to confirm an action, in which case it's often called a 'dialog box'.

However, modals are deceptively tricky, they have complex usability expectations and accessibility requirements.

##### Building a modal

In the sandbox below, you have an incomplete `Modal` component. Your job is to see how many issues you can find + fix.

[Sandbox Code - Modal](https://codesandbox.io/s/kgtwu5?file=/Modal.js&utm_medium=sandpack)

- When opening the modal, focus state should be on the modal, in this case, the close button.
- Do this by applying a `useEffect` within the `Modal` component to apply the `.focus()` on the button element. Or whatever element you want, using the `useRef`.

```JAVASCRIPT
import React from 'react';
import { X as Close } from 'react-feather';

import styles from './Modal.module.css';

function Modal({ handleDismiss, children }) {
  // Get button reference
  const closeBtnRef = React.useRef();

  console.log(closeBtnRef.current);

  // Apply the focus state for keyboard navigation
  React.useEffect(() => {
    closeBtnRef.current.focus();
  }, [])

  return (
    <div className={styles.wrapper}>
      <div className={styles.backdrop} />
      <div className={styles.dialog}>
        <button
          // apply ref
          ref={closeBtnRef}
          className={styles.closeBtn}
          onClick={handleDismiss}
        >
          <Close />
        </button>
        {children}
      </div>
    </div>
  );
}

export default Modal;
```

- The next thing, is when you close the modal, to return the focus state, to the previously focused element used to launch the modal.
- You can use an existing property for this, `document.activeElement`, this will tell you which element is currently focused as you browse the page/DOM.
- With that, you can store that in a variable, to use within our `useEffect`. To restore focus when the component unmounts. And add a cleanup function to re-focus the element.

```JAVASCRIPT
import React from 'react';
import { X as Close } from 'react-feather';

import styles from './Modal.module.css';

function Modal({ handleDismiss, children }) {
  // Get button reference
  const closeBtnRef = React.useRef();

  console.log(closeBtnRef.current);

  // Apply the focus state for keyboard navigation
  React.useEffect(() => {
    // Store previously focused element
    const currentlyFocusedElement = document.activeElement;

    // Focus on the close button on mount
    closeBtnRef.current.focus();

    // Cleanup function, when component unmounts
    // Add the optional chaining operator ?, that way if wasn't anything focused, don't do anything at all
    return () => {
      currentlyFocusedElement?.focus();
    }
  }, []);

  return (
    <div className={styles.wrapper}>
      <div className={styles.backdrop} />
      <div className={styles.dialog}>
        <button
          // apply ref
          ref={closeBtnRef}
          className={styles.closeBtn}
          onClick={handleDismiss}
        >
          <Close />
        </button>
        {children}
      </div>
    </div>
  );
}

export default Modal;
```

- The next issue, when the modal is launched, you can still tab through and into the page in the background.
- How do you lock focus withing a container, element? With this one, can use a third party library, `react-focus-lock`
- How does it work? [react-focus-lock](https://www.npmjs.com/package/react-focus-lock)

```JAVASCRIPT
 import FocusLock from 'react-focus-lock';

 const JailForAFocus = ({onClose}) => (
    <FocusLock>
      You can not leave this form
      <button onClick={onClose} />
    </FocusLock>
 );
```

- This will make it possible to navigate inside the modal.

```JAVASCRIPT
import React from 'react';
import { X as Close } from 'react-feather';
// import the focus trap
import FocusLock from 'react-focus-lock';

import styles from './Modal.module.css';

function Modal({ handleDismiss, children }) {
  // Get button reference
  const closeBtnRef = React.useRef();

  console.log(closeBtnRef.current);

  // Apply the focus state for keyboard navigation
  React.useEffect(() => {
    // Store previously focused element
    const currentlyFocusedElement = document.activeElement;

    // Focus on the close button on mount
    closeBtnRef.current.focus();

    // Cleanup function, when component unmounts
    // Add the optional chaining operator ?, that way if wasn't anything focused, don't do anything at all
    return () => {
      currentlyFocusedElement?.focus();
    }
  }, []);

  return (
    <FocusLock>
    <div className={styles.wrapper}>
      <div className={styles.backdrop} />
      <div className={styles.dialog}>
        <button
          // apply ref
          ref={closeBtnRef}
          className={styles.closeBtn}
          onClick={handleDismiss}
        >
          <Close />
        </button>
        {children}
      </div>
    </div>
    </FocusLock>
  );
}

export default Modal;
```

- Another issue, is you can scroll the background page behind the modal. We can use another library by the same developer to solve this, [react-remove-scroll](https://www.npmjs.com/package/react-remove-scroll).

```JAVASCRIPT
import React from 'react';
import { X as Close } from 'react-feather';
// import the focus trap
import FocusLock from 'react-focus-lock';
// remove scroll
import {RemoveScroll} from 'react-remove-scroll';

import styles from './Modal.module.css';

function Modal({ handleDismiss, children }) {
  // Get button reference
  const closeBtnRef = React.useRef();

  console.log(closeBtnRef.current);

  // Apply the focus state for keyboard navigation
  React.useEffect(() => {
    // Store previously focused element
    const currentlyFocusedElement = document.activeElement;

    // Focus on the close button on mount
    closeBtnRef.current.focus();

    // Cleanup function, when component unmounts
    // Add the optional chaining operator ?, that way if wasn't anything focused, don't do anything at all
    return () => {
      currentlyFocusedElement?.focus();
    }
  }, []);

  return (
    <FocusLock>
      <RemoveScroll>
        <div className={styles.wrapper}>
          <div className={styles.backdrop} />
          <div className={styles.dialog}>
            <button
              // apply ref
              ref={closeBtnRef}
              className={styles.closeBtn}
              onClick={handleDismiss}
            >
              <Close />
            </button>
            {children}
          </div>
        </div>
      </RemoveScroll>
    </FocusLock>
  );
}

export default Modal;
```

- Another thing, when you cannot click on the outside element to close the modal. We need to fix this.
- For this, just add an `onClick` to the backdrop element and pass along the `handleDismiss` function.
- You can just add it to the `div`, not an issue since it's redundant functionality, you already have the button for the close.

```JAVASCRIPT
  return (
    <FocusLock>
      <RemoveScroll>
        <div className={styles.wrapper}>
          <div onClick={handleDismiss} className={styles.backdrop} />
          <div className={styles.dialog}>
            <button
              ref={closeBtnRef}
              className={styles.closeBtn}
              onClick={handleDismiss}
            >
              <Close />
            </button>
            {children}
          </div>
        </div>
      </RemoveScroll>
    </FocusLock>
  );
```

- Another common thing, is to hit the `esc` key to close. We need to add that as well.
- This requires another `useEffect`, to listen for the keyboard, and also to cleanup the event

```JAVASCRIPT
  // add another effect for the keyboard
  React.useEffect(() => {
    function handleKeyDown(event) {
      if (event.code === `Escape`) {
        handleDismiss();
      }
    }

    // add the listener for keyboard
    window.addEventListener('keydown', handleKeyDown);

    // cleanup function to undo the event listener
    return () => {
      window.removeEventListener('keydown', handleKeyDown);
    };

  }, [handleDismiss]);
```

- Screen reader issues, they don't know what the context is.
- Use the `VisuallyHidden` component for the close button on a screen reader.
- For the dialog, add `role="dialog" aria-modal="true" aria-label={title}`, and pass in the title as a prop.

```JAVASCRIPT
  return (
    <FocusLock>
      <RemoveScroll>
        <div className={styles.wrapper}>
          <div 
            onClick={handleDismiss} 
            className={styles.backdrop} 
          />
          <div 
            className={styles.dialog}
            role="dialog"
            aria-modal="true"
            aria-label={title}
          >
            <button
              ref={closeBtnRef}
              className={styles.closeBtn}
              onClick={handleDismiss}
            >
              <Close />
              <VisuallyHidden>
                Dismiss Modal
              </VisuallyHidden>
            </button>
            {children}
          </div>
        </div>
      </RemoveScroll>
    </FocusLock>
  );
```

- Full Modal Component and forked solution

- [Forked Solution](https://codesandbox.io/s/modal-accessibility-react-y6dr3c)

```JAVASCRIPT
import React from 'react';
import { X as Close } from 'react-feather';
// import the focus trap
import FocusLock from 'react-focus-lock';
// remove scroll
import {RemoveScroll} from 'react-remove-scroll';

import styles from './Modal.module.css';
import VisuallyHidden from './VisuallyHidden';

function Modal({ title, handleDismiss, children }) {

  // add the ref
  const closeBtnRef = React.useRef();
  console.log(closeBtnRef);

  // create a useEffect, that will only run when the component mounts
  React.useEffect(() => {
    // run this
    // capture to refocus when closed
    const currentlyFocusedElem = document.activeElement;

    // focus the close btn
    closeBtnRef.current.focus();

    // cleanup function, refocus
    return () => {
      currentlyFocusedElem?.focus();
    }
  }, []);

  // add another effect for the keyboard
  React.useEffect(() => {
    function handleKeyDown(event) {
      if (event.code === `Escape`) {
        handleDismiss();
      }
    }

    // add the listener for keyboard
    window.addEventListener('keydown', handleKeyDown);

    // cleanup function to undo the event listener
    return () => {
      window.removeEventListener('keydown', handleKeyDown);
    };

  }, [handleDismiss]);

  return (
    <FocusLock>
      <RemoveScroll>
        <div className={styles.wrapper}>
          <div 
            onClick={handleDismiss} 
            className={styles.backdrop} 
          />
          <div 
            className={styles.dialog}
            role="dialog"
            aria-modal="true"
            aria-label={title}
          >
            <button
              ref={closeBtnRef}
              className={styles.closeBtn}
              onClick={handleDismiss}
            >
              <Close />
              <VisuallyHidden>
                Dismiss Modal
              </VisuallyHidden>
            </button>
            {children}
          </div>
        </div>
      </RemoveScroll>
    </FocusLock>
  );
}

export default Modal;
```

#### Exercises -

##### Hamburger Navigation

- The hamburger menu is a common convention, clicking or tapping the hamburger menu icon will open a nav menu. Common on mobile and can be found on desktop.

- In the sandbox is an incomplete hamburger menu. Your job is to finish implementing it, taking care to consider usability and accessibility of this feature.

ACs:

- While the menu is open, focus should be trapped within the modal (you can use the provided `FocusLock` component)
- When the menu is closed, focus should be restored to the last-focused element.
- While the menu is open, scrolling should be disabled (you can use the provided `RemoveScroll` component)
- Hitting 'Escape' should close the menu.
- Clicking the backdrop should close the menu.
- The markup should be structured correctly, for screen-reader users. Her are some links to help out.
  - [ARIA menu button pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menu-button/)
  - [Example HTML](https://www.accede-web.com/en/guidelines/rich-interface-components/hamburger-menu/)

- ℹ️ Note: We do not want to automatically focus the dismiss button when the menu opens, like we did in the previous Modals lesson.

- [Forked Code Sandbox - Hamburger Menu](https://codesandbox.io/s/hamburger-menu-exercise-react-csqcsg?file=/Header.js)

- Note on Accessibility, reference markup examples from this resource, [AcceDe Web](https://www.accede-web.com/en/guidelines/rich-interface-components/hamburger-menu/)

```HTML
<nav role="navigation" aria-label="Main menu">
  <button aria-expanded="true">
    <svg aria-hidden="true" focusable="false">[…]</svg>
    Menu
  </button>
  <ul class="visible">
    [Main navigation menu]
  </ul>
</nav>
```

- Full Drawer Component:

```JAVASCRIPT
import React from "react";
import { X as Close } from "react-feather";
import FocusLock from "react-focus-lock";
import { RemoveScroll } from "react-remove-scroll";

import styles from "./Drawer.module.css";

function Drawer({ handleDismiss, children }) {
  // Create a hook for the keyboard, see below
  useEscapeKey(handleDismiss);

  return (
    <FocusLock>
      <RemoveScroll>
        <div className={styles.wrapper}>
          <div onClick={handleDismiss} className={styles.backdrop} />
          <div className={styles.drawer}>
            <div>{children}</div>
            <button className={styles.closeBtn} onClick={handleDismiss}>
              <Close size={18} /> Dismiss
            </button>
          </div>
        </div>
      </RemoveScroll>
    </FocusLock>
  );
}

// Hook for the keyboard functionality
function useEscapeKey(callback) {
  // Effect to capture and refocus element
  React.useEffect(() => {
    // store previously focused element
    const currentlyFocusedElem = document.activeElement;

    // cleanup and refocus when closed
    return () => {
      currentlyFocusedElem?.focus();
    };
  }, []);

  // Effect to capture the keyboard actions
  React.useEffect(() => {
    // keydown function
    function handleKeydown(event) {
      if (event.code === "Escape") {
        callback();
      }
    }

    // listener for the keyboard
    window.addEventListener("keydown", handleKeydown);

    // cleanup function
    return () => {
      window.removeEventListener("keydown", handleKeydown);
    };
  }, [callback]);
}

export default Drawer;
```
