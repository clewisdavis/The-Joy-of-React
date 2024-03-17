# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Baked in Laziness

Earlier we discussed teh concept of producers and consumers.

- Producing, is the act of building somethign that developers wil use to build products. For example, we might buidl a generic `Button` component.
- Consuming, it the act of using that thing. For example, sprinking the `Buttton` component all over the application, creating the actual UI that users will use.

The past few lessons, looking at how to implement lazy-loading, and we have been thinking about it exclusively form the consumer perspective. When we want to use a component, we have to swap out the import.

```JAVASCRIPT
// From this...
import HeavyComponent from '@/components/HeavyComponent';

// ...to this:
const HeavyComponent = dynamic(() => import('@/components/HeavyComponent'));
```

This places the burden on the consumer, if we use the `HeavyComponent` 10 differ times in our app, we have to remember to use teh alternative `import()` syntax each and every time.

Instead, I prefer to 'bake in' the lazy loading on the producer side. That way, we gaurantee that the compoennt will always be lazy-loaded, without the consumer needing to do anything special.

See how we can do this with our `react-latex-next` package.

### Creating a component wrapper

The immediate problem, is we are not the producer of the `react-latex-next` package. We didn't create it, not our code.

We will solve for that by building our own thin wrapper around this package:

```JAVASCRIPT
// src/components/Latex.js
import Spinner from '@/components/Spinner';

const LazyLatex = dynamic(
  () => import('react-latex-next'),
  { loading: Spinner }
);

function Latex({ children, ...delegated }) {
  return (
    <LazyLatex {...delegated}>
      {children}
    </LazyLatex>
  );
}

export default Latex;
```

- This new `Latex` component is a thin wrapper around `react-latex-next`. We forward all of the props along, so that it can be used exactly the same as the underlying third party component.

- When we need to format some LaTeX, we will import this new component, using a standard import.

```JAVASCRIPT
import Latex from '@/components/Latex';
```

- By creating this thin wrapper around the `react-latex-next` package. We have taken the mantle of 'producer'.
- This new `Latex` component is our own creation, and we can use it to ock in whatever we want. In this cre, we have implemented a full lazy-loading setup, including the loading indicator.

- We have removed the burden from the consumer. When we are in 'consumer mode' we dont' have to do anythig special to get al the benefits of lazy loading. It is baked in. Cool! 😄

ℹ️ The facade pattern - The word “facade” typically refers to the face of a building, the part you actually see as a pedestrian from the street. The building itself is much larger and more complex, but we see a nice friendly exterior.

- Huge benefit to using the facade pattern: we can swap one NPM package out for another.
