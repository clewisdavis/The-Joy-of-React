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
- Then you can use this in the `<App>` component, and if you wanted to just show, if you wanted to just show something all the time. Whether they are logged in or not.

```JAVASCRIPT
// BANNER
import React from 'react';

import Banner from './Banner';

const EXAMPLE_USER = {
  name: 'Sayen',
  registrationStatus: 'verified',
};

function App() {
  return (
    <Banner
      type="success"
    >
      Email to reset password sent!
    </Banner>
    
  );
}

export default App;
```

- But, if you wanted to conditionally show the `Banner` based on log in.
- Import your `LoggedInBanner` and use that in the place of `Banner`

```JAVASCRIPT
import React from 'react';

import { LoggedInBanner } from './Banner';

const EXAMPLE_USER = {
  name: 'Sayen',
  registrationStatus: 'verified',
};

function App() {
  return (
    <LoggedInBanner
      type="success"
      user={EXAMPLE_USER}
    >
      Successfully logged in! Welcome aboard, {EXAMPLE_USER.name}!
    </LoggedInBanner>
    
  );
}

export default App;
```

- Now you have two different components, that occupy different parts of the spectrum.
- `Banner`, is the primitive, and `LoggedInBanner` is the one with business logic.

![Spectrum](images/image-25.png)

- Benefits, you have these two differ components you can use depending on which banner is most appropriate.
- The real benefit, is the structure of your application.
- The problem is, when you cram all the logic in the low level component, it's impossible to maintain or follow. Early returns, conditions, etc.
- And every time the product changed, a new requirement, you end up cramming all that logic into one area of the app. Cannot tell what that part of the app is rendering.

- When you restructure it, it becomes very clear what each component does and renders.
- 📣 And as the product changes and scales, you can solve that by creating new components.
- The cool thing, is all the new components you are spinning off, all use the same underlying banner.

### Exercises

#### Product Details Page

Imagine you are building nan e-commerce site. You have received a mockup, and we implemented as one big component.

In terms of our spectrum of abstraction, we have created a very high  level component, but there are two lower level components hiding within. Begging to be extracted into their own components.

Your mission (is you choose it) is to identify a spectrum related problem and to extract at least 1 lower level component.

- AC's
  - Extract at least 1 lower-level, potentially-reusable component.
  - The extracted component can be defined inside `/ProductDetails.js`, below the main component.
  - If you are not sure where to start / which components should be extracted, I provide my suggestions below the sandbox.

👩‍💻 Code Sandbox - [Product Details Page](https://codesandbox.io/s/rp4htb?file=/ProductDetails.js&utm_medium=sandpack)

- Attempted to extract the `buttons`, within the photo carousel. And create a new component called `PhotoButtons`.
- Passing in as props, product, and the state, `selectedPhotoIndex` and `setSelectedPhotoIndex`

```JAVASCRIPT
// My attempt to abstract the photos button. 
function PhotoButtons({ product, selectedPhotoIndex, setSelectedPhotoIndex }) {
  return (
          <div className="buttons">
            {product.photos.map((photoSrc, index) => {
              const isSelected = selectedPhotoIndex === index;
  
              return (
                <button
                  key={index}
                  className="thumbnail-button"
                  onClick={() => setSelectedPhotoIndex(index)}
                >
                  <VisuallyHidden>
                    Toggle image #{index + 1}
                  </VisuallyHidden>
                  <img alt="" src={photoSrc} />
                  <span
                    className="selected-ring"
                    style={{
                      opacity: isSelected ? 1 : 0,
                    }}
                  />
                </button>
              );
            })}
          </div>  
  )
}
```

- Solution: Try and do the star rating, because it's a low level component with a bunch of logic in it.
- Pass in the `product` as a prop.

```JAVASCRIPT
// Star rating
function StarRating({ product }) {
  return (
        <div className="star-rating">
          {range(5).map((num) => {
            const className =
              product.rating > num
                ? 'star filled'
                : 'star hollow';
            return <Star className={className} />;
          })}
        </div>
  )
}
```

- You can take this a step further, by updating the prop name, to `rating`, and just pass in the product.rating to make it clear you are only using a single object.

```JAVASCRIPT
function ProductDetails({ product }) {
    return (
        <StarRating 
          rating={product.rating}
        />
        // rest of component code
    )
}
// Star rating component
function StarRating({ rating }) {
  return (
        <div className="star-rating">
          {range(5).map((num) => {
            const className =
              rating > num
                ? 'star filled'
                : 'star hollow';
            return <Star className={className} />;
          })}
        </div>
  )
}
```

- Another thing you can pull out, is the photo toggle functionality.
- And move the state, inside that component, since it's not used anywhere else.

```JAVASCRIPT
function PhotoToggle({ photos }) {
  // move the state, inside the photos toggle
    const [
    selectedPhotoIndex,
    setSelectedPhotoIndex,
  ] = React.useState(0);
  
  return (
      <div>
          <img
            className="primary-photo"
            alt=""
            src={photos[selectedPhotoIndex]}
          />        
          <div className="buttons">
            {photos.map((photoSrc, index) => {
              const isSelected = selectedPhotoIndex === index;
  
              return (
                <button
                  key={index}
                  className="thumbnail-button"
                  onClick={() => setSelectedPhotoIndex(index)}
                >
                  <VisuallyHidden>
                    Toggle image #{index + 1}
                  </VisuallyHidden>
                  <img alt="" src={photoSrc} />
                  <span
                    className="selected-ring"
                    style={{
                      opacity: isSelected ? 1 : 0,
                    }}
                  />
                </button>
              );
            })}
          </div>  
      </div>
  )
}
```

- Then take a look at the higher level component, much easier to see what is going on.
- In the props, you can see at a high level, what the data structure each component uses.

```JAVASCRIPT
// Uses the low level components, StarRating and PhotoToggle defined above. 
function ProductDetails({ product }) {

  return (
    <article className="product-details">
      <div className="photos-wrapper">
        <div>
          <PhotoToggle 
            photos={product.photos}
          />
        </div>
      </div>
      <div className="product-info">
        <h1>{product.title}</h1>
        <StarRating 
          rating={product.rating} 
        />
        <p className="product-description">
          {product.description}
        </p>
      </div>
    </article>
  );
}
```

- Full Solution of `ProductDetails.js`
- Code Sandbox - [Product Component Spectrum](https://codesandbox.io/s/magical-microservice-td2ghh?file=/ProductDetails.js)

![Product Spectrum](images/image-26.png)

```JAVASCRIPT
// ProductDetails.js
import React from 'react';
import { Star } from 'react-feather';

import { range } from './utils';
import VisuallyHidden from './VisuallyHidden';

function ProductDetails({ product }) {

  return (
    <article className="product-details">
      <div className="photos-wrapper">
        <div>
          <PhotoToggle 
            photos={product.photos}
          />
        </div>
      </div>
      <div className="product-info">
        <h1>{product.title}</h1>
        <StarRating 
          rating={product.rating} 
        />
        <p className="product-description">
          {product.description}
        </p>
      </div>
    </article>
  );
}

function StarRating({ rating }) {
  return (
        <div className="star-rating">
          {range(5).map((num) => {
            const className =
              rating > num
                ? 'star filled'
                : 'star hollow';
            return <Star className={className} />;
          })}
        </div>
  )
}

function PhotoToggle({ photos }) {
  // move the state, inside the photos toggle
    const [
    selectedPhotoIndex,
    setSelectedPhotoIndex,
  ] = React.useState(0);
  
  return (
      <div>
          <img
            className="primary-photo"
            alt=""
            src={photos[selectedPhotoIndex]}
          />        
          <div className="buttons">
            {photos.map((photoSrc, index) => {
              const isSelected = selectedPhotoIndex === index;
  
              return (
                <button
                  key={index}
                  className="thumbnail-button"
                  onClick={() => setSelectedPhotoIndex(index)}
                >
                  <VisuallyHidden>
                    Toggle image #{index + 1}
                  </VisuallyHidden>
                  <img alt="" src={photoSrc} />
                  <span
                    className="selected-ring"
                    style={{
                      opacity: isSelected ? 1 : 0,
                    }}
                  />
                </button>
              );
            })}
          </div>  
      </div>
  )
}

export default ProductDetails;
```

#### Extracting a Card Component

In the sandbox below, we have two separate components that each implement a similar 'card' design. Let's extract into it's own `Card` component.

- 👩‍💻 Code sandbox - [Card Component](https://codesandbox.io/s/xrdbfl?file=/App.js&utm_medium=sandpack)

AC's

- Two files, `Card.js` and `Card.module.css`, have been created. Your mission is to populate them with the component + styles.
- You should use this component within `UserProfileCard` and `ProductInfoCard`.
- Note that the two cards have slightly differ styles: they have a different `box-shadow` value. Each card should be able to specify the elevation level for the shadow, to be applied dynamically inside `Card`.

- Create a `Card` component, with a `children` prop so you can use it in the other components `UserProfileCard` and `ProductInfoCard`

```JAVASCRIPT
import React from 'react';

import styles from './Card.module.css';

function Card({ children }) {
  /* TODO */
  return (
    <div className={styles.card}>
      {children}
    </div>
  )
}

export default Card;
```

- Make sure you bring over the base `.card` styles into your CSS module `Card.module.css`

```CSS
.card {
  background: white;
  padding: 16px;
  border-radius: 8px;
}
```

- Now you need to account for the different elevations within the CSS global variables, `var(--elevation-1)`, `var(--elevation-2)`, `var(--elevation-3)`.

- Add an `elevation` prop to your `Card` component
- Create a boxShadow variable, and interpolate the CSS variable using the value of the prop `elevation`
- And add it to the DOM element via the `style` attribute.

```JAVASCRIPT
import React from 'react';

import styles from './Card.module.css';

function Card({ 
  elevation = 2, // values 1 | 2 | 3
  children,
}) {
  /* TODO */
  // Create a style for the elevation prop, and interpolate into a string using the CSS variable
  const boxShadow = `var(--elevation-${elevation})`;
  return (
    <div className={styles.card} style={{ boxShadow }}>
      {children}
    </div>
  )
}

export default Card;
```

- Then, on your other components, import and then wrap with `Card` component. You can just pass in the elevation value you want, 1, 2, or 3.

```JAVASCRIPT
function UserProfileCard({ user }) {
  const profileUrl = `/users/${user.handle}`;
  const imageAlt = `${user.avatarDescription} (user profile photo)`;

  return (
    <>
    <Card elevation={1}>
      <img
        className={styles.avatar}
        alt={imageAlt}
        src={user.avatarSrc}
      />
      <a
        href={profileUrl}
        className={styles.userProfileLink}
      >
        {user.name}
      </a>
    </Card>
      
    </>
  );
}

export default UserProfileCard;
```
