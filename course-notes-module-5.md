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
