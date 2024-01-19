# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Portals

Portals allows you to 'teleport' the output of a DOM node to a target container. This is useful when creating and resolving UI elements that overlay on the screen, for example a modal etc.

Syntax example:

```JAVASCRIPT
import { createPortal } from 'react-dom';

function Modal({ children }) {
  return createPortal(
    // The React elements to render:
    <div className="modal">
      {children}
    </div>,
    // The target DOM container to hold the output:
    document.querySelector('#modal-root')
  );
}
```

- You will also need to edit the HTML file to create the target container.

```JAVASCRIPT
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <div id="root"></div>
    <div id="modal-root"></div>
  </body>
</html>
```

## Exercise, ToastShelf Portal

In previous exercises, we used built a generic `Toast` component from scratch. In addition, we create da `ToastShelf` component, responsible for displaying a list of toasts in the bottom right corner of the page.

Let's use a portal to ensure that the `ToastShelf` component always renders without issue.

ACs:

- The `ToastShelf` component shoudl use the `createPortal` function from React DOM to render it's contents (the `<ol>`) in a different root node.
- You will need to add this new root to the `index.html` file.

- Solution Notes:

In the index.html, just add a `id='root-toast'` do a new `<div>`, so the portal knows where to go.

```HTML
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Toast Example</title>
  </head>
  <body>
    <div id="root"></div>
    <div id="root-toast"></div>
  </body>
</html>
```

- In the `ToastShelf` component, add the `createPortal` return.

```JAVASCRIPT
import React from 'react';
import { createPortal } from 'react-dom';

import { ToastContext } from '../ToastProvider';
import Toast from '../Toast';
import styles from './ToastShelf.module.css';

function ToastShelf() {
  const { toasts } = React.useContext(ToastContext);

  return createPortal (
    // React element to render
    <ol
      className={styles.wrapper}
      role="region"
      aria-live="assertive"
      aria-label="Notification"
    >
      {toasts.map((toast) => (
        <li key={toast.id} className={styles.toastWrapper}>
          <Toast id={toast.id} variant={toast.variant}>
            {toast.message}
          </Toast>
        </li>
      ))}
    </ol>,
    // target DOM node
    document.querySelector('#root-toast')
  );
}

export default ToastShelf;
```
