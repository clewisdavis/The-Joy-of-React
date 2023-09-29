# The Joy of React - Module 4 - Course Notes

- [Course Outline Notes](course-notes.md)

## Component API Design

### Introduction

- Most common question, 'How do I create a React apps that scale?"

- Going to learn techniques that keep complexity manageable as apps grow larger and larger.
- Gain new mental models, ways of thinking that will help make architectural decisions in our apps.

- Producers and consumers
- Think of this, the restaurant analogy, the waiter is your 3rd party API, and the waiter is the interface that you use to access you meal. You don't see all that goes on in the kitchen.

- Twitter for example, teams of people just working on that API layer, they decide what the parameters should be, the routes, authentication, permissions.
- Producing and consuming, Twitter developers produce this API, and we consume it.
- Common pattern, every time we install an NPM package, we are consuming something another developer has produced.

- A React component is also like a closed system, like the Twitter example. A React component closes up the markup and styles, we don't have access to that directly, instead, we have access to the interface, which is props.

- As the person developing the components, we can control what props are included and excluded. And we don't think about it. In a React producer and consumer, we wear both hats.

- It may take some time to produce the component, but for years, you will be consuming it.
- If the props, are intuitive and easy to use, it will be easy to use.

- If a component is designed well, produced in a thoughtful manner, the prop interface, will be nice to work with in the future.

### The Spectrum of Components

- Here is a simple implementation of a `<Banner/>` component, meant to show teh user a message.

```JAVASCRIPT
import React from 'react';

import styles from './Banner.module.css';

function Banner({ type, user, children }) {
  const backgroundColor = type === 'success'
    ? 'var(--color-success)'
    : 'var(--color-error)';
  
  // Only logged in, verified users are
  // allowed to see the banner
  if (
    !user ||
    user.registrationStatus === 'unverified'
  ) {
    return null;
  }
  
  return (
    <div
      className={styles.banner}
      style={{ backgroundColor }}
    >
      {children}
    </div>
  );
}

export default Banner;
```

- What do you think about this structure?
- An opportunity to do things, in a more flexible and scalable way.

- The spectrum of components.
- On the left, primitive lego bricks we build our application with, reusable and generic.

![spectrum](images/image-23.png)

- But we need things that are more application specific and tied into our business logic.
- As you move along the spectrum we start to be more and more tied into our specific application.

![Alt text](images/image-24.png)

- At the end of the spectrum, the components that are the most tied into our application and business logic.

- Good mental model, every component should be clear where they sit on the spectrum.

- With this in mind, where should this go in our spectrum? How would you structure this?
- Whenever you render our <LoggedInBanner/> it does the business logic check, then it defers to our generic <Banner/>.

```JAVASCRIPT
// Create a new component
// this component can be responsible for the logic
// and then you can return a generic <Banner/>
function LoggedInBanner({ type, user, children }) {
  // Only logged in, verified users are
  // allowed to see the banner
  if (
    !user ||
    user.registrationStatus === 'unverified'
  ) {
    return null;
  }  

  return (
    <Banner type={type}>
      {children}
    </Banner>
  )
}
```

- And now, you have two separate components, that have two separate concerns.
- Purely cosmetic, based on it's type.

```JAVASCRIPT
// Generic Banner
function Banner({ type, children }) {
  const backgroundColor = type === 'success'
    ? 'var(--color-success)'
    : 'var(--color-error)';
   
  return (
    <div
      className={styles.banner}
      style={{ backgroundColor }}
    >
      {children}
    </div>
  );
}
```

- You can export `<LoggedInBanner>`, so you can use it in your `<App>`.
