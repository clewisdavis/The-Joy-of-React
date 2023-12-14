# The Joy of React - Module 5 - Course Notes

- [Course Outline Notes](course-notes.md)

## Best Practices

- Do best practices exists, real world is full of nuance and issues. Something that works in one situation doesn't fit so well in another.
- In this module, build upon what already learned and share some of the design patterns and best practices. How to apply them in similar situations and the trade offs.
- Dig deeper into how React works and clearer mental model for things like keys and refs.

## Leveraging Keys

- In the playground, what if you want to trigger a simple CSS keyframe animation? You have a component and a form, that updates based on some state.
- Your first instinct, might be to reach for some conditional logic, based on state and to update with the `useEffect`. But you might not need an Effect.
- See this document from React - [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect)
- An Effect is good for synchronizing some external system or non-React widget.

- In previous lessons, we used `key` to identify an element as we mapped over it. To remove the `key` warning.
- Keys allow React to connect a specific ID with a specific React element and the associated DOM.
- So when that data changes in the key `key={price}` it triggers a re-render of that element and DOM manipulation.

- This is much easier than trying to write conditional logic and an Effect, just to trigger a CSS animation.

```JAVASCRIPT
// PriceDisplay.js
import React from 'react';
import styles from './PriceDisplay.module.css';

function PriceDisplay({ price }) {
  const formattedPrice = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
  }).format(price);
  
  return (
    <div className={styles.wrapper}>
      <div key={price} className={styles.animated}>
        {formattedPrice}
      </div>
    </div>
  );
}

export default PriceDisplay;
```

### Exercises

#### Pricing Issues

In the previous exercise, has one potential issue. If multiple plans have the same price, the price will not animate:

![Pricing Issues](images/image-36.png)

- Update the sandbox below so that the price animates when the user changes plans, even if those plans are the same price.

ACs:

- The display price should animate whenever the user changes the plan, even if the price doesn't change.

Exercise Notes:

- We can be very strategic how we use keys, and choose a value, that updates all the time.
- In this case, the `price` value doesn't change, but the `id` is unique for each item in the `PLANS` object.
- To make the animation fire, use the `plan.id` and pass it down to the `<PriceDisplay>` component.

```JAVASCRIPT
  <PriceDisplay id={plan.id} price={plan.price} />
```

- Then within your `<PriceDisplay>` component, add `id` to the props and use the `key={id}` on the element you want to React to redraw, if that value is updated.

```JAVASCRIPT
import React from 'react';
import styles from './PriceDisplay.module.css';

// ADD THE ID
function PriceDisplay({ id, price }) {
  const formattedPrice = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
  }).format(price);
  
  // ADD IT TO THE ELEMENT YOU WANT TO UPDATE
  return (
    <div className={styles.wrapper}>
      <div key={id} className={styles.animated}>
        {formattedPrice}
      </div>
    </div>
  );
}

export default PriceDisplay;
```

- [Sandbox Environment Solution](https://codesandbox.io/p/sandbox/keys-update-dom-pj3c7l?file=%2Findex.js)

#### Coin Clicker, revisited

Earlier in the course, we created this 'Coin' clicker, to learn how to lift state up. Now, let's suppose we want to add a little '+2' that shows whenever the coin is clicked.

The markup has been provided, your job is to re-trigger the animation whenever the coin is clicked.

![Coin](images/image-37.png)

ACs:

- Clicking the coin should show the +2 animation.
- The animation should not show when the page originally loads. It should only show when the number of coins changes.

- Notes:

- In the previous exercise, we set the `key` on an actual DOM element within the JSX. But you can also set a `key` on the component itself, to trigger a re-render.
- Here, when the state of `numOfCoins` is updated, we update the `<FloatingText>` component by setting a `key` value on it.

```JAVASCRIPT
    <FloatingText key={numOfCoins}>
      + {numOfCoins}
    </FloatingText>
```

- This will re-render the component, therefore triggering the +2 animation.
- The next problem, is to add a condition, so the animation doesn't run on initial page load.

```JAVASCRIPT
        {/* Conditionally run this, to prevent the animation from running on page load */}
        {numOfCoins > 0 && (
          <div className={styles.floatingNumWrapper}>
            <FloatingText key={numOfCoins}>
              + {numOfCoins}
            </FloatingText>
          </div>        
        )
        }
```

- Use the logical `&&` operator to check the condition `numOfCoins > 0`, boolean value, if it's true, it will render the `<FloatingText>` component.

- [Code and Box Playground - Animation Leveraging Keys](https://codesandbox.io/p/sandbox/leveraging-keys-coin-clicker-animation-lw75tq)
