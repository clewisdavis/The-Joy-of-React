# Module 2 - Working with State

- [Course Outline Notes](course-notes.md)

## Dynamic Key Generation

- When iterating over data with `.map`, we nee dto give each Rect element a unique `key` attribute so that React knows which DOM operations to trigger between renders.

- In previous exercises, the data conveniently came with unique tokens for each item, and we used those tokens:

```JAVASCRIPT
const inventory = [
  {
    id: 'abc123',
    name: 'Soft-boiled egg press',
  },
  {
    id: 'def456',
    name: 'Hello Kitty toothbrush',
  },
];

// We can use the `id` field in our iterations:
inventory.map(item => (
  <ShopItem key={item.id} item={item} />
));
```

- But what if our data doesn't have a unique token we can use?

- This is one of the most common questions that developers have. React needs a unique value for each item, but we don't have one.
- Sometimes we can use the array index for the key, but this approach doesn't always work. How can we solve this problem?

### Stickers example

- This example, when you click to produce random stickers:
- The core logic is done, but we get an warning in the console. Let's fix it.

- The goal is to fix the key warning by dynamically generating the keys for each sticker. We don't want to use the array index this time around.

```JAVASCRIPT
// starter code
import React from 'react';

import styles from './StickerPad.module.css';
import { getSticker } from './Stickers.data';

function StickerPad() {
  const [stickers, setStickers] = React.useState([]);

  return (
    <button
      className={styles.wrapper}
      onClick={(event) => {
        const stickerData = getSticker();
        const newSticker = {
          ...stickerData,
          x: event.clientX,
          y: event.clientY,
        };

        const nextStickers = [...stickers, newSticker];
        setStickers(nextStickers);
      }}
    >
      {stickers.map((sticker) => (
        <img
          className={styles.sticker}
          src={sticker.src}
          alt={sticker.alt}
          style={{
            left: sticker.x,
            top: sticker.y,
            width: sticker.width,
            height: sticker.height,
          }}
        />
      ))}
    </button>
  );
}

export default StickerPad;
```

- In order for React to work in the optimal way, it needs a unique identifier for each of the images

```JAVASCRIPT
   <img
     // need to set a key
     key={}
     className={styles.sticker}
     src={sticker.src}
     alt={sticker.alt}
     style={{
       left: sticker.x,
       top: sticker.y,
       width: sticker.width,
       height: sticker.height,
     }}
   />
```

- We don't have a unique identifier in the data file. You can try to use the src.

```JAVASCRIPT
const STICKERS = [
  {
    src: 'https://sandpack-bundler.vercel.app/img/stickers/cactus.svg',
    alt: 'potted cactus sticker',
    width: 295 / 2,
    height: 452 / 2,
  }
]
```

- You can try to use the source `key={sticker.src}`, but the moment you have two of the same, you get an key error in the console.
- Or you could try and dynamically generate a string based on multiple properties, this is better but you can still get key errors.

```JAVASCRIPT
  key={`${sticker.src}-${sticker.x}-${sticker.y}`}
```

- What if you try and use a random value, `key={Math.random}`, seems to work, you don't get any warnings. But still not a good idea. The reason is performance. Think about the role of the key, to tell React how things should change and update.

```JAVASCRIPT
   <img
     // need to set a key
     key={Math.random()}
     className={styles.sticker}
     src={sticker.src}
     alt={sticker.alt}
     style={{
       left: sticker.x,
       top: sticker.y,
       width: sticker.width,
       height: sticker.height,
     }}
   />
```

- Every time you give a new key, you are telling React to re-render that element in the DOM. Even though you didn't need to, or nothing changed, all the same properties.
- When you use `Math.random` as the `key` it is destroying all the DOM elements and creating new ones each time. Over time this can cause inefficiencies and performance issues.

- This concept is a good idea, to generate a unique value each time
- The problem is, we don't want to do it in the render, on the element, every time we update the stickers array.

- What if we did it on the handler, `onClick` handler.
- On the `newSticker` object, we set `id: Math.random()`
- Then on the `img` element, we set the key to `key={sticker.id}`

- If you console.log the `stickers` object, you will see the id, and how it does not change when you add new stickers, the id persists.
- It generates the id, only on the `onClick`, when you first generate the sticker. Every other time, is just uses the static value of `key={sticker.id}`
- We have create a persistent id, a real static value, and React will know, if it has to do any work to re-render or not.

```JAVASCRIPT
  return (
    <button
      className={styles.wrapper}
      onClick={(event) => {
        const stickerData = getSticker();
        const newSticker = {
          ...stickerData,
          x: event.clientX,
          y: event.clientY,
          // Add to the handler, generate a unique id
          id: Math.random(),
        };

        const nextStickers = [...stickers, newSticker];
        setStickers(nextStickers);
      }}
     >
      {stickers.map((sticker) => (
        <img
          // Use that previously created unique value for teh key:
          // Apply it to the element:
          key={sticker.id}
          className={styles.sticker}
          src={sticker.src}
          alt={sticker.alt}
          style={{
            left: sticker.x,
            top: sticker.y,
            width: sticker.width,
            height: sticker.height,
          }}
        />
      ))}
    </button>
  );
```

- You can use other methods, to generate a unique id, `Date.now()`, or `crypto.randomUUID()` 🤔 this is not cryptography not cryptocurrency 😄, this generates a UUID, really long.

- This is the strategy, create a unique identifier when you create the object, you can use this when fetching data, the moment you get that data back, you can add unique identifiers to the object
