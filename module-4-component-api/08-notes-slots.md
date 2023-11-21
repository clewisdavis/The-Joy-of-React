# The Joy of React - Module 4 - Component API Design

- [Course Outline Notes](../course-notes.md)

## Component API Design

### Slots

- You can use slots to pass any markup to a component. An alternative to having to define a bunch of props, and think of every case.
- We have been using slots all along, the `{children}` prop for example.

```JAVASCRIPT
function YourComponent({ children }) {
  return (
    <div>{children}</div>
  )
}
```

- Imagine you want to render a photo, and use the `<picture>` tag to render the correct density photo for the device. You end up with a lot of image sources, hard to maintain through props.

```CODE
<picture>
  <source
    type="image/webp"
    srcSet={`
      https:/img/spaceship.webp 1x,
      https:/img/spaceship@2x.webp 2x,
      https:/img/spaceship@3x.webp 3x
  `}
  />
  <source
    type="image/png"
    srcSet={`
      https:/img/spaceship.png 1x,
      https:/img/spaceship@2x.png 2x,
      https:/img/spaceship@3x.png 3x
  `}
  />
  <img
    alt="A meerkat looking curiously at the camera"
    src="/img/meerkat.jpg"
  />
</picture>
```

- A better pattern is to use the **slots pattern**. Here is what it looks like:

```JAVASCRIPT
function CaptionImage({ image, caption }) {
  return (
    <figure>
      {image}
      <div className="divider" />
      <figcaption>{caption}</figcaption>
    </figure>
  );
}
```

- And the consumer would use it like this:

```JAVASCRIPT
<CaptionedImage
  image={
    <img
      alt="A punk-rock cat illustration"
      src="https://sandpack-bundler.vercel.app/img/punk-cat.png"
    />
  }
  caption="Illustration by Josh Comeau"
/>
```

- The `image` prop takes a React element. It creates a 'slot' inside the `<figure>` for the consumer to include whatever markup we need.

- We have been doing this all along, with the `{children}` prop, not a new idea.
- The `children` prop isn't special, we can pass React elements to any prop, not just `{children}`.

- 🤔 The cool thing is, we can pass multiple slots in a single component. It's like being able to specify multiple distinct children.

- For example, we can pass a `caption` and a `image` prop.

```JAVASCRIPT
<CaptionedImage
  image={
    <img
      alt="A meerkat looking curiously at the camera"
      src="https://sandpack-bundler.vercel.app/img/meerkat.jpg"
    />
  }
  caption={
    <>
      Photo by <a href="">Manuel Capellari</a>, shot in August 2019 and
      published on <strong>Unsplash</strong>.
    </>
  }
/>
```

- And then specify the slots in the component.

```JAVASCRIPT
function CaptionedImage({ image, caption }) {
  return (
    <figure>
      {image}         {/* <— Slot 1 */}
      <div
        className="divider"
      />
      <figcaption>
        {caption}     {/* <— Slot 2 */}
      </figcaption>
    </figure>
  );
}
```

- 📣 This pattern provides maximum control and power to the consumer:

- With delegated props, we are able to provide additional props to a particular element.
- With polymorphism, we are able to change a particular element's HTML tag.
- With slots, able to provide any markup we want, without restrictions.

- 🤔 This is a tradeoff, the more power you give to the consumer, the more flexible our component is, but more likely things can go awry. The consumer can potentially introduce bugs, use the component in ways not intended.

- Code Sandbox - [Slots](https://codesandbox.io/s/0ulfh7?file=/App.js&utm_medium=sandpack)

#### Exercises, Slots

Exercises for Slots

##### Icon Buttons

Build an `IconButton` component.

![Icon Button](images/image-34.png)

- Will use the `react-feather` package to provide the icons. Here is how we render an icon from `react-feather`.

```JAVASCRIPT
import { Award } from 'react-feather';

<Award size={32} />
```

ACs:

- We should be able to pass any icon we want, as a React element, to the button. We can test using the imported icons from the `react-feather` package.
- The `IconButton` component should render the icon in the appropriate slot.

- My solution:

- First, design the consumer experience for the component, how you would like to pass in the properties.

```JAVASCRIPT
import React from 'react';
import {
  Award,
  Camera,
  Frown,
  Slash,
  XCircle,
} from 'react-feather';

import IconButton from './IconButton';

function App() {
  // TODO: Render an “IconButton
  // Add an 'icon' prop and just pass in the icon
  // as a component, <Award/>, literally
  return (
  <>
    <IconButton icon={<Award/>}>
      Win an award
    </IconButton>
    <IconButton icon={<Camera/>}>
      Take a picture
    </IconButton>

  </>
  )
}

export default App;
```

- Then, design the component with the correct props to support the consumer experience.

```JAVASCRIPT
import React from 'react';

import styles from './IconButton.module.css';

// Add the props, children and icon and include the slots
// inside the JSX
function IconButton({ children, icon }) {
  return (
    <button className={styles.wrapper}>
      <span className={styles.iconWrapper}>
        { icon }   {/* <— Slot for icon */}
      </span>
      <span className={styles.childrenWrapper}>
        { children }  {/* <— Slot for children */}
      </span>
    </button>
  );
}

export default IconButton;
```

##### Stretch Goal, to much control

The problem here, is we have given to much control over the `icon`.

ACs:

- The `IconButton` should still have an icon prop, which gives it an icon to render. `IconButton` should not be the one importing the icons from 'react-feather'.
- `IconButton` should have 100% control over the props being applied to the icon.

- Solution: Passing the component through the `icon` prop, rather than an element in App.js

```JAVASCRIPT
// App.js
import React from 'react';
import {
  Award,
  Camera,
  Frown,
  Slash,
  XCircle,
} from 'react-feather';

import IconButton from './IconButton';

function App() {
  return (
    <>
      <IconButton icon={Award}>
        Collect Award
      </IconButton>
      <IconButton icon={Frown}>
        Rate Our Product
      </IconButton>
      <IconButton icon={XCircle}>
        Dismiss
      </IconButton>
    </>
  );
}

export default App;
```

- Then we render the component inside `IconButton`.

```JAVASCRIPT
// IconButton.js
import React from 'react';

import styles from './IconButton.module.css';

// rename the icon prop using destructuring
function IconButton({ icon: Icon, children }) {
  return (
    <button className={styles.wrapper}>
      <span className={styles.iconWrapper}>
        <Icon size={20} strokeWidth={1.5} />
      </span>
      <span className={styles.childrenWrapper}>
        {children}
      </span>
    </button>
  );
}

export default IconButton;
```

ℹ️ Icons in React, collection

- [Feather Icons](https://feathericons.com)
- [Lucide Icons](https://lucide.dev)
