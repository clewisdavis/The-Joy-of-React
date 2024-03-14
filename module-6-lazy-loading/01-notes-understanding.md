
# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Understanding lazy loading

- 🤔 The big idea with lazy loading is that we will defer loading the JS code associated with a particular component until it's needed.

- This isn't a new idea, been part of React API for a few years, `React.lazy()` method.

- Next.js has their own wrapper around `React.lazy()`, for now it will be helpful to reiew `React.lazy()`.
- Looks like:

```JAVASCRIPT
// Instead of this:
import Latex from 'react-latex-next';

// ...do this:
const Latex = React.lazy(() => import('react-latex-next'));
```

- `Latex` is the React component, now the difference is, it will be lazy loaded.
- This works by using teh dynamic `import()` syntax.
- These are resolved by the Webpack bundler when we compile our code.
- It signals to the bundler that it should split off a new bundle, `react-latex-next`, and any sub-dependencies, will not be included.

Look at an example:

```JAVASCRIPT
'use client';
import React from 'react';

const Latex = React.lazy(() => import('react-latex-next'));

function MathQuestion() {
  const [showMath, setShowMath] = React.useState(false);

  return (
    <>
      <button onClick={() => setShowMath(true)}>
        Reveal equation
      </button>

      {showMath && <Latex>{'$2^4 - 4$'}</Latex>}
    </>
  );
}

export default MathQuestion;
```

- In this scenario, the `showMath` state variable is used to conditionally render math notation.
- By defualt, `showMath` is `false`, so we don't render our `<Latex>` element. User has to click a button to toggle it on.

- 🤔 The whole idea with lazy loading is that we defer loading the code until it's needed. Because we are not rending `<Latex>` initially, no need to download the `react-latex-next` module code.

- The result, when the user visits our app, the bundle isn't downloaded, saving 72kb of JS from the initial page load.
- But, we still have to download the 72kb if we want to render a math notation.

- When click the button to show, flip the `showMath` flips to `true`, react wil say, 'hold up, I don't have this component' and request from the server, to fetch the 72kb bundle. Then it will re-render.

- However, we are given no indication this is happening. Can solve this will 'Suspense'.
- This will render a spinner while the math-rendering code is downloaded and fetched.

```JAVASCRIPT
'use client';
import React from 'react'

import Spinner from '@/components/Spinner';

const Latex = React.lazy(() => import('react-latex-next'));

function MathQuestion() {
  const [showMath, setShowMath] = React.useState(false);

  return (
    <>
      <button onClick={() => setShowMath(true)}>
        Reveal equation
      </button>

      <React.Suspense fallback={<Spinner />}>
        {showMath && <Latex>{'$2^4 - 4$'}</Latex>}
      </React.Suspense>
    </>
  );
}
```

- This works because `React.lazy()` is a Suspense-compatible wrapper around the underlying component.
- When `Latex` is rendered for the first time, react realizes that we don't actually have the component. So it does, throw a promise' to suspend this portion of the React tree.

- When the data is received, and everything is ready, React re-renders using the real `Latex` component and this portion of the app un-suspends.
