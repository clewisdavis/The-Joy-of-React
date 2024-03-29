# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Understanding Suspense

In the previous lessons, we saw how Suspense can offer dramatically improved loading experiences. But what exactly is Suspense?

In order to answer this question, we need to go on a bit of a journey.

![spinner hell](images/image-3.png)

As each section loads, it shoves everything around, leading to a poor User Experience. React is built this way, each component is packaging up all the stuff for that particular UI, if each component fetches it own data, we wind up in 'spinner hell'

Look at a code example;

```JAVASCRIPT
function TrafficCard() {
  const { data, isLoading } = useSWR('/api/traffic', fetcher);

  if (isLoading) {
    return <Spinner />;
  }

  return <Card>{/* Stuff using `data` */}</Card>;
}

function OfferCard() {
  const { data, isLoading } = useSWR('/api/offer', fetcher);

  if (isLoading) {
    return <Spinner />;
  }

  return <Card>{/* Stuff using `data` */}</Card>;
}

function Dashboard() {
  return (
    <>
      <TrafficCard />
      <OfferCard />
    </>
  );
}
```

- When we render the `<Dashboard />` component, we start off with two loading spinners. Both `TrafficCard` and `OfferCard` make network request to fetch their data. When that data comes through, the component re-renders the real UI.

- For example, initial load...

![loading](images/image-4.png)

- `Offer` component loads...

![offer](images/image-5.png)

- Then the `Traffic` component loads...

![traffic](images/image-6.png)

- The trouble is that each component is operating independently. They both make a request for data, and when received, they re-render immediately.

- The more data-fetching elements we have, the more potential for chaos. The FB Ads Manager has at atleast 6 individual data-fetching components, and looks like this...

![Alt text](images/image-7.png)

- The technical term for when things move around like this; layout shift.

![shift](images/image-8.png)

- See this article - [Cumulative Layout Shifts (CLS)](https://web.dev/articles/cls)

**So how do we fix this?**

- We could fix this by lifting the data-fetching requests up to the parent component, so that we can 'group' their loading states. Something like this?

```JAVASCRIPT
function Dashboard() {
  const {
    data: trafficData,
    isLoading: trafficIsLoading,
  } = useSWR('/api/traffic', fetcher);
  const {
    data: offerData,
    isLoading: offerIsLoading,
  } = useSWR('/api/offer', fetcher);

  // If *either* request is still pending, we'll show a spinner:
  if (trafficIsLoading || offerIsLoading) {
    return <Spinner />
  }

  return (
    <>
      <TrafficCard data={trafficData} />
      <OfferCard data={offerData} />
    </>
  )
}
```

- In this new version, we show a spinner until both network request have resolved.

![Alt text](images/image-9.png)

- This is a better user experience. By waiting until both components are ready, we avoid the jarring experience of elements jumping around.

- But it feels like a step backwards in terms of the developer experience. Components should own their own data request, the same way they own their styles, markup, and business logic.

**What if there was a way for us to keep our original modular component structure, but to strategically “group” UI updates to avoid excessive layout shifts?**

🚀 This is the problem Suspense was originally designed to solve.

### Introducing the Suspense Component

Start with some code example.

```JAVASCRIPT
import React from 'react';

function Dashboard() {
  return (
    <React.Suspense fallback={<Spinner />}>
      <TrafficCard />
      <OfferCard />
    </React.Suspense>
  );
}
```

`Suspense` is a special React component that can help us orchestrate loading states. In this case, we are saying that `<TrafficCard>` and `<OfferCard>` are part of the same 'group'.

We specify out loading state with the `fallback` prop. React will render this fallback until all of its children have finished loading.

This produces the same user experience as our 'lifting fetch up' approach.

![suspense](images/image-10.png)

How does this work?

- `<React.Suspense>` component is only half of the story. The other half of the Suspense API si that the components have to signal whether they are ready or not; they need to be able to say 'Hey, don't render yet! I am still loading my data'.

- A good analogy is a concert, the rock show cannot start until all the band members are present.
- The name 'Suspense' comes from, React will suspend rendering until all of the children have finished loading. The curtains will stay closed until every one is ready.

![Alt text](images/image-11.png)

**How do individual components signal their loading state?**

- React has very little insight into what actaully happens inside our components.
- This Fetch API for example;

```JAVASCRIPT
function TrafficCard() {
  const [data, setData] = React.useState(null);

  React.useEffect(() => {
    fetch('/api/traffic')
      .then((res) => res.json())
      .then((data) => {
        setData(data);
      });
  }, []);

  return <Card>{/* Stuff here */}</Card>;
}
```

- In this example; the only thing React knows is that we have some sort of side effect.
- React cannot see into effects, the Fetch API is part of the web platform, not part of React, so React doesn't even know that a network request is happening!

- `<React.Suspense>` has no way of knowing that `TrafficCard` is loading. We need a way to explicitly trigger the 'suspend' during teh first render. And then signal that component is ready once the data has been loaded.

- **This part, suspend, is done by library authors, not app developers.**

### A new kind of boundary

In previous modules, we learned about Error Boundaries, We wrap an `<Error Boundary>` component around a slice of the React tree, and it will catch any errors thrown by any descendants, to keep the app from blowing up.

![Error Boundary](images/image-12.png)

As a refresher, the code for this setup looks like this:

```JAVASCRIPT
function App() {
  return (
    <>
      <Header />
      <ErrorBoundary fallback="Something went wrong…">
        <RealTimeInfo />
      </ErrorBoundary>
      <Stories />
    </>
  );
}
```

- With this error boundary in position, any of the coloured components, `RealTimeInfo`, `Ticker`, and `Price`, can throw an error, and it will cause the error boundary to swap out the `<RealTimeInto>` element for the provided `fallback`, usually an error message.

- `<React.Suspense>` also creates a boundary, but one that catches loading states instead of errors.
- We provide a loading fallback instead of an error fallback, and the component automatically swaps between them as-necessary.

![Suspense Boundary](images/image-13.png)

- Like error boundaries, Suspense boundaries will catch loading states in any descendants, not only the direct children.
- If `SearchForm` signals to the Suspense boundary that it's loading, this entire slice of the tree will be suspended, and we will show the fallback instead.

- The really wild part, descendants communicate their loading state by broadcasting it using the `throw` keyword. But instead of throwing an error, we throw a fetch request itself.

- Really technical done under the hood, will never actually have to `throw` a promise in your work.
