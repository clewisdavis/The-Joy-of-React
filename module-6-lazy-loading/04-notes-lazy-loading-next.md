# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Lazy Loading in Next

The `React.lazy()` method works in all React contexts, including Next.js. However, there is a more conventional way to use this in Next apps.

- It has a familiar API:

```JAVASCRIPT
import dynamic from 'next/dynamic';

const Latex = dynamic(() => import('react-latex-next'));
```

- Under the hood, this `dynamic` function uses `React.lazy`. It is a drop in replacement for it, and works in the same way. But by switching to `dynamic` we get a couple small bells and whistles.

### Build in loading state

With `React.lazy`, we specify loading states by wrapping the lazy component in `<React.Suspense>`.

```JAVASCRIPT
const Latex = React.lazy(() => import('react-latex-next'));

<React.Suspense fallback={<Spinner />}>
  {showMath && <Latex>{'$2^4 - 4$'}</Latex>}
</React.Suspense>
```

WIth `dynamic` we can pass a second argument for configuration. One of the config options is a loading component.

```JAVASCRIPT
const Latex = dynamic(
  () => import('react-latex-next'),
  { loading: Spinner }
);

<Latex>{'$2^4 - 4$'}</Latex>
```

- They `dynamic` function wraps up the `<React.Suspense>` element for us, so we can 'back in' a loading state for this component.

- This isn't necessarily better. With `React.lazy`, we have more control, since we are the ones that place the Suspense boundary. This allows us to 'batch' multiple lazy loaded components under one loading state.

```JAVASCRIPT
const Latex = React.lazy(() => import('react-latex-next'));
const Fireworks = React.lazy(() => import('some-fireworks-package'));

function App() {
  const [showLatex, setShowLatex] = React.useState(false);

  return (
    <>
      <React.Suspense fallback={<Spinner />}>
        <Latex />
        <Fireworks />
      </React.Suspense>
      <button onClick={() => setShowLatex(!showLatex)}>
        Toggle
      </button>
    </>
  );
}
```

- In this scenario, we have two large third party components: `<Latex>` and `<fireworks>`.

### Disabling SSR

Next gives us the option to disable server side rendering for lazy loaded components.

```JAVASCRIPT
const Fireworks = dynamic(() => import('../components/fireworks'), {
  ssr: false,
  loading: Spinner,
});
```

- Wen we do this, Next will render the fallback component during the server side render. The initial HTML will contain a spinner, and it will be swapped out on the client once the bundle is loaded.

- We generally don't want to do this, good thing we can include these heavy component in our HTML w/o affecting the initial JS bundle.

- But sometimes we will find working with third party components that just don't work on the server.

- In the 'SSR Gotchas' lessons, we saw a valid example of that, `window.localStorage.getItem()` will crash the server, since is not 'window' object in Node.js

- The `dynamic` function gives us a tool we can use to skip rendering a component on the server, and only on the client.

- You can learn more about '[Lazy Loading](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading)' in Next.js docs
