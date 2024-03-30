# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Using Suspense in Next

In the [Sole and Ankle example](https://courses.joshwcomeau.com/joy-of-react/06-full-stack-react/09.01-exciting-new-world), the primary way to access Suspense in Next is through the `loading.js`.

- When we create a file named `loading.js` and colocate it next to `page.js`, Next will automatically wrap the two into a Suspense boundary. Somewhere in Next.js codebase, likely some code that looks like this.

```JAVASCRIPT
function NextApp({ params, LoadingComponent, PageComponent }) {
  if (LoadingComponent) {
    return (
      <React.Suspense
        fallback={<LoadingComponent params={params} />}
      >
        <PageComponent params={params} />
      </React.Suspense>
    )
  }

  return (
    <PageComponent params={params} />
  );
}
```

- By creating a `loading.js` component, we opt into Suspense.

- Hang on a second, there are two parts of the Suspense API.

1. Creating a Suspense Boundary with `<React.Suspense>`.
2. Broadcasting the loading status for components within the boundary.

Creating the boundary is only half the story. We also need a way for the components within that boundary to say 'Hey, don't render yet! I am still waiting on data'.

- How do we do that in Next.js? Do we need to throw a promise somewhere?

**Fortunately not.** React Server Components is here to save the day.

- Take a look at the `page.js` component from that project:

```JAVASCRIPT
// /src/app/shop/[categorySlug]/page.js
import React from 'react';

import { getShoesForCategory } from '@/helpers/data';
import ShoeGrid from '@/components/ShoeGrid';

async function CategoryPage({ params }) {
  const shoes = await getShoesForCategory(params.categorySlug);

  return <ShoeGrid shoes={shoes} />;
}

export default CategoryPage;
```

- This component is a Server Component, since it doesn't include the 'use client' directive. This means it only ever renders on the server.

- In a lot of ways, Server Components are simpler than Client Components. They never re-render; after the first render, they are done.
- Server Components are also allowed to be `asynchronous`. Notice that we are using the `async` keyword here, in the function definition. This means that when we render this component, we get back a 'Promise', and that promise will resolve after the data-fetching is complete and the component has been rendered.

🤯**With Server Component, React can use the component itself to track whether the component is ready or not.**

🚀 TAKEAWAY: When we use React Server Components with Suspense, things 'just work'. We don't have to use any special libraries, we don't have to `throw` anything. When we put an async Server Component within a Suspense boundary, react will suspend until it's finished rendering.
