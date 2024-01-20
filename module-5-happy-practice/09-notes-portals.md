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

## Exercise, Portal Component

Let's do something fun and create a `Portal` component. This time instead of editing the HTML to include `<div id="tost-root">` our `Portal` component will create this node dynamically. When the component mounts it should create a brand new DOM node, and use it on the portal's root node.

ACs:

- The `Portal` component should use the `createPortal` function from React DOM to render its `children` in a portal.
- When the component is unmounted, it should clean up after itself; any dynamically created nodes should be destroyed.
- You will know it's working when the inner box is teleported outside its parent.

- Solution notes:

```JAVASCRIPT
import React from 'react';
import { createPortal } from 'react-dom';

function Portal({ children }) {
  // create a state variable and set to null
  const [host, setHost] = React.useState(null);

  // add a useEffect hook
  React.useEffect(() => {
    // create the DOM element
    const host = document.createElement('div');
    // append it to the body DOM node
    document.body.appendChild(host);
    // add an attribute to it, just to help identify for future reference
    host.setAttribute('data-react-portal-host', '')

    // set the state variable
    setHost(host);

    //clean up old elements
    return () => {
      host.remove();
    };
  }, [])

  // condition to check for host and bail out early
  if (!host) {
    return null;
  }

  // render the host
  return createPortal(children, host);
}

export default Portal;
```

This seems really complicated and why would I need to use this?

- Sandbox - [React Portals](https://codesandbox.io/p/sandbox/react-portals-hxq78l?layout=%257B%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522rootPanelGroup%2522%253A%257B%2522direction%2522%253A%2522horizontal%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522id%2522%253A%2522ROOT_LAYOUT%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522clrm6m9ri00063b5vdwa3290u%2522%252C%2522sizes%2522%253A%255B70%252C30%255D%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522EDITOR%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522id%2522%253A%2522clrm6m9ri00023b5vp1boxrnk%2522%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522SHELLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522id%2522%253A%2522clrm6m9ri00033b5vt2l17nm3%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522DEVTOOLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522id%2522%253A%2522clrm6m9ri00053b5vfkk9tkpt%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%252C%2522sizes%2522%253A%255B50%252C50%255D%257D%252C%2522tabbedPanels%2522%253A%257B%2522clrm6m9ri00023b5vp1boxrnk%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clrm6m9ri00013b5vor3q2wlu%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522FILE%2522%252C%2522filepath%2522%253A%2522%252Findex.js%2522%252C%2522state%2522%253A%2522IDLE%2522%257D%255D%252C%2522id%2522%253A%2522clrm6m9ri00023b5vp1boxrnk%2522%252C%2522activeTabId%2522%253A%2522clrm6m9ri00013b5vor3q2wlu%2522%257D%252C%2522clrm6m9ri00053b5vfkk9tkpt%2522%253A%257B%2522id%2522%253A%2522clrm6m9ri00053b5vfkk9tkpt%2522%252C%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clrm6m9ri00043b5v0w94hlhc%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522UNASSIGNED_PORT%2522%252C%2522port%2522%253A0%252C%2522path%2522%253A%2522%252F%2522%257D%255D%252C%2522activeTabId%2522%253A%2522clrm6m9ri00043b5v0w94hlhc%2522%257D%252C%2522clrm6m9ri00033b5vt2l17nm3%2522%253A%257B%2522tabs%2522%253A%255B%255D%252C%2522id%2522%253A%2522clrm6m9ri00033b5vt2l17nm3%2522%257D%257D%252C%2522showDevtools%2522%253Atrue%252C%2522showShells%2522%253Atrue%252C%2522showSidebar%2522%253Atrue%252C%2522sidebarPanelSize%2522%253A15%257D)
