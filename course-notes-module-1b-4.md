# Module 1 - Components / Styling

- [Course Outline Notes](course-notes.md)

## Styling in React

- React is un-opinionated when it comes to styling. As a results, dozens of options when it comes to using CSS with React.

- Technically, you can do things the traditional way. Bunch of CSS files and incudes them as a `<link>` in your `index.html`.

- 🤔 Thinking in Components
- The core idea with components is that each component is a bundle of markup (in JSX)
  - Logic in JS
  - Styles in CSS
  - Markup in HTML

- For example, our `button` component should "own" all the the styles related to that component.
- Working with CSS becomes much simpler.

#### CSS Modules

- Let' build a component, called "Side note", a call out to hold tangential information.
- Here is a rough sketch of the markup

```JAVASCRIPT
function Sidenote({ title, children }) {
  return (
    <aside>
      <h3>{title}</h3>
      <p>{children}</p>
    </aside>
  )
}
```

- And here is how to solve it with CSS modules:

```JAVASCRIPT
// sidenote.js
import styles from './Sidenote.module.css';

function Sidenote({ title, children }) {
  return (
    <aside className={styles.wrapper}>
      <h3 className={styles.title}>{title}</h3>
      <p>{children}</p>
    </aside>
  );
}

export default Sidenote;
```

```CSS
/* Sidenote.module.css */
.wrapper {
  padding: 24px;
  background-color: hsl(210deg 55% 92%);
  border-left: 3px solid hsl(245deg 100% 60%);
  border-radius: 3px 6px 6px 3px;
}

.title {
  font-size: 1rem;
  font-weight: bold;
  margin-bottom: 4px;
}
```

- It is strange that you can import a css file, from a JS file. This is being done with the power of tooling, webpack
- Also, note how super short and generic, `.wrapper` for example is typically used throughout an application. A top level element.
- Surely you cannot just name things `.wrapper`, it will collide with other styles named `.wrapper`.
- The power of CSS modules, in the example above, log out styles and see what you get.
- What you get is an object where the keys, are the short names you defined in the `CSS` file.

```JAVASCRIPT
{
  "wrapper": "_components_Sidenote_module__wrapper",
  "title": "_components_Sidenote_module__title",
  "button": "_components_Sidenote_module__button"
}
```

- However, notice they have a prefix, of a bunch of stuff. Where is this coming from?
- 🚀 This prefix, is what **guarantees the uniqueness**.
- If you inspect you element, you will see the final output, of the class names, are prefixed.

```HTML
<aside class="_components_Sidenote_module__wrapper">
  <h3 class="_components_Sidenote_module__title">I'm a sidenote!</h3>
  <p>I contain some text!</p>
</aside>
```

- In `CSS` file, we write short names and the build process expands them out.
- This is done with WebPack toolchain.
- When we import, and write `.module.css`, this has a meaning in WebPack.
- It tells Webpack to
  - Create longer names for each style.
  - Inject them into the `<head>` of our document.
  - And producing the `styles` object, so we can use the styles in our component `className={styles.wrapper}`

- We get all the benefits of the naming convention BEM, without having to manually do it. All the hard stuff just happens.
- How is the prefix defined? It is using the file path, and converting it into the prefix. `/components/Sidenote.module.css` gets converted to the prefix for each style. `_components_Sidenote_module__wrapper`
- **You will never have a collision, because you can never have two files with the same name.**

- Other tools do the same thing, like `styled components` but has a learning curve.
- The nice thing about CSS Modules, it's a lot like writing vanilla CSS.

#### Exercises, CSS Modules

##### Sidenote types

- 4 different 'types' of sidenote components, depending on the status of things happening.
- In this exercise, update the Sidenote component to support a new `type` prop. This prop will be used to apply a dynamic CS class.

- This is the markup we want to end up with:

```JAVASCRIPT
<aside class="wrapper notice">
  <h2>This is an informational sidenote.</h2>
</aside>

<aside class="wrapper success">
  <h2>This is a success sidenote!</h2>
</aside>
```

- The `wrapper` class contains a shared style. The `type` class like `notice`, `success` contains teh category specific styles.

- In this exercises, you will implement this pattern using CSS modules. The task is to apply the correct class name base on the `type` prop.

- AC's
- All sidenotes should have the `wrapper` class applied.
- They `type` property of the Sidenote should apply an additional class, which changes the color of the background and border.

- 📣 Never rely on color alone, it is not enough. Use another visual indicator.

```JAVASCRIPT
// Sidenote.js
import styles from './Sidenote.module.css';

function Sidenote({ type, title, children }) {
  return (
    <aside>
      <h3 className={styles.title}>{title}</h3>
      <p>{children}</p>
    </aside>
  );
}

export default Sidenote;
```

```CSS
/*
  These are the "universal" styles which should
  apply to all Sidenote instances.
*/
.wrapper {
  padding: 24px;
  border-left: 3px solid;
  border-radius: 3px 6px 6px 3px;
  margin-bottom: 32px;
}

/*
  These styles are specific for the given category.
*/
.notice {
  background-color: hsl(210deg 55% 92%);
  border-color: hsl(245deg 100% 60%);
}
.warning {
  background-color: hsl(38deg 100% 50% / 0.1);
  border-color: hsl(30deg 100% 50%);
}
.error {
  background-color: hsl(340deg 95% 43% / 0.1);
  border-color: hsl(340deg 95% 60%);
}
.success {
  background-color: hsl(160deg 100% 40% / 0.1);
  border-color: hsl(160deg 100% 40%);
}

.title {
  font-size: 1rem;
  font-weight: bold;
  margin-bottom: 4px;
}
```

- To add multiple classes, use a template string literal to combine, concatenate.
- The store that into a variable, and pass into your `className={sideNoteClasses}` with an expression slot.

```JAVASCRIPT
import styles from './Sidenote.module.css';

function Sidenote({ type, title, children }) {
  const sideNoteClasses = `${styles.wrapper} ${styles[type]}`;
  return (
    <aside className={sideNoteClasses}>
      <h3 className={styles.title}>{title}</h3>
      <p>{children}</p>
    </aside>
  );
}

export default Sidenote;
```

- How does that style get generated?

- ℹ️ When to use square brackets?
- What is the difference between `styles[type]` and `styles.type`. When do we use dot and when do we use square brackets?
- When we use square brackets, we create an "expression slot". We can put any JS expression we want in there, and it will resolve that expression to a key.

- Dot notation has become the standard way of doing things if we know the exact name of they key we want to look up.
- But most times, it's tucked away in a variable:

```JAVASCRIPT
function getMessage(type) {
  return messages[type];
}
```

- We don't know the value of `type`. We need to resolve the variable, since `type` itself doesn't tell us anything.
- **Square brackets allow us to resolve the variable.**

##### Movie rating animation

- For movies that are 9.0 and higher, we want to add an animation to the rating number.
- Apply the class `glowingReview` to all the ratings that are 9 and above.

- Starter code:

```JAVASCRIPT
import React from 'react';

import styles from './Movie.module.css';

// Your mission:
// Apply the 'glowingReview' CSS class to the
// movie rating when it's 9 or higher.

function Movie({ movie }) {
  return (
    <article className={styles.movie}>
      <div className={styles.thumbnailWrapper}>
        <img
          alt="Movie poster"
          src={movie.posterSrc}
        />
      </div>
      <div className={styles.textWrapper}>
        <h2>{movie.title}</h2>
        <p className={styles.synopsis}>
          {movie.synopsis}
        </p>
        <p>
          <strong>Rating:</strong> {movie.rating}
        </p>
      </div>
    </article>
  );
}

export default Movie;
```

- My Attempt:
- I used the Logical AND `&&` operator on the rating to display the class. Then passed that into an expression slot.

```JAVASCRIPT
import React from 'react';

import styles from './Movie.module.css';

// Your mission:
// Apply the 'glowingReview' CSS class to the
// movie rating when it's 9 or higher.

function Movie({ movie }) {
// USED THE LOGICAL &&, PASSED IT TO THE SPAN BELOW
  const review =
    movie.rating > 9 && `${styles.glowingReview}`;

  return (
    <article className={styles.movie}>
      <div className={styles.thumbnailWrapper}>
        <img
          alt="Movie poster"
          src={movie.posterSrc}
        />
      </div>
      <div className={styles.textWrapper}>
        <h2>{movie.title}</h2>
        <p className={styles.synopsis}>
          {movie.synopsis}
        </p>
        <p>
          <strong>Rating: </strong>
          <span className={review}>
          {' '}{movie.rating}
          </span>
        </p>
      </div>
    </article>
  );
}

export default Movie;
```

- How does that work, if `movie.rating > 9` then it will render the `${styles.glowingReview}`l
- If it is less than 9, it will resolve to false, and not render anything, and React is smart enough to not render anything. So you will see just a `<span>` with the rating.
- React is smart enough, to not render anything for `false` or `null` or `undefined`, it will not render the `className` at all.  

- However you do get a warning in the console. Warning: Received `false` for a non-boolean attribute `className`.

- A better solution is to use a `ternary` expression instead.

```JAVASCRIPT
<span
  className={movie.rating >= 9 ? styles.glowingReview : undefined}
>
  {movie.rating}
</span>
```

- ℹ️ Class utilities: The `className` prop expects a string, this string can include multiple classes, separated by spaces.
- We can dynamically create this string using String Interpolation.

```JAVASCRIPT
<button
  className={`
    ${styles.btn}
    ${type === 'primary' ? styles.primary : ''}
    ${user ? styles.loggedIn : styles.notLoggedIn}
  `}
>
```

- This works but tricky. In cases like this, easier to use a class utility
- [clxs package](https://www.npmjs.com/package/clsx)

```JAVASCRIPT
import clsx from 'clsx';

<button
  className={clsx(
    styles.btn,
    type === 'primary' && styles.primary,
    user ? styles.loggedIn : styles.notLoggedIn
  )}
>
```

- The `clsx` function will take each of these arguments and produce a unified string that satisfies teh `className` prop requirements. And automatically remove falsy values like `false` or `null`.

- That's a wrap for React Fundamentals.
