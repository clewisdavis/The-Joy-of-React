# Module 1 - Components / Conditional Rendering

- [Course Outline Notes](course-notes.md)

## Conditional Rendering

- Most of the time, we want to render something based on a condition.
- For example, render a green dot next to an avatar if a user is online.

#### With an If Statement

- With the curly brackets, we can add JS expressions within our JSX
- But, we CANNOT add JS statements.
- For example; this is not allowed.

```JAVASCRIPT
function Friend({ name, isOnline }) {
  return (
    <li className="friend">
      {if (isOnline) {
        <div className="green-dot" />
      }}

      {name}
    </li>
  );
}
```

- Why doesn't this work? Consider how this will compile to JS:

```JAVASCRIPT
function Friend({ name, isOnline }) {
  return React.createElement(
    'li',
    { className: 'friend'},
    if (isOnline) {
      React.createElement('div', { className: 'green-dot' });
    },
    name
  );
}
```

- you can't put an `if` statement in teh middle of a function call like this. Here is a more simple example:

```JAVASCRIPT
console.log(
  if (isLoggedIn) {
    "Logged in!"
  } else {
    "Not logged in"
  }
)
```

- 🤔 TIP: Want to know if a piece of JS is an expression or statement? Try to log it out!
- If it runs, the code is an expression.
- If you get an error, it's a statement, or possibly invalid JS syntax.

```JAVASCRIPT
console.log(/* Some chunk of JS here */);
```

- In React, we are allowed to put *expressions* in our JSX, but NOT *statements*.
- Expression: JS code that produces a value. `5 * 10` produces `50`. `num > 100` produces either `true` or `false`
- Statement: An instruction to the computer to do a thing. `let hi = 5;` or `if (hi > 10) { // stuff here }`

- Good news; we can still use an `if` statement. But we have to pull it up so that it's not it in the middle of a `React.createElement` call:

```JAVASCRIPT
// component Friend.js
function Friend({ name, isOnline }) {
  let prefix;

  if (isOnline) {
    prefix = <div className="green-dot" />;
  }

  return (
    <li className="friend">
      {prefix}
      {name}
    </li>
  );
}

export default Friend;
```

- No rule that says your JSX has to be part of the `return` statement.
- We can assign chunks of JSX to a variable, anywhere within oru component definition.

- This JSX compiles to:

```JAVASCRIPT
function Friend({ name, isOnline }) {
  let prefix;

  if (isOnline) {
    prefix = React.createElement(
      'div',
      { className: 'green-dot' },
    );
  }

  return React.createElement(
    'li',
    { className: 'friend' },
    prefix,
    name,
  );
}
```

- In the example above, `prefix` will either be assigned a React element or it won't be defined at all.
- **This works because React ignores children that don't produce a value.**

- Another example;

```JAVASCRIPT
// Example 1:
function SomeBoringComponent() {
  let someClass;

  return (
    <div className={someClass}>
      {undefined}
    </div>
  );
}

// Renders an empty div:
// <div></div>
```

- 📣 React ignores falsy values, including `undefined`, `null`, `false`. Because of this, we can use conditional rendering to dynamically add/remove elements.

#### With &&

- The downside to using an `if` statement is we have to pull that logic up, away from the rest of the markup.
- Makes it harder to understand hwo a component is structured. Having to hop all over the place to understand what is being returned.

- Good news, a way to embed the `if` logic right in our JSX, using the `&&` operator.

```JAVASCRIPT
function Friend({ name, isOnline }) {
  return (
    <li className="friend">
      {isOnline && <div className="green-dot" />}
      {name}
    </li>
  );
}

function App() {
  return (
    <ul className="friend-list">
      <Friend name="Andrew" isOnline={false} />
      <Friend name="Beatrice" isOnline />
      <Friend name="Chen" isOnline />
    </ul>
  );
}

export default App;
```

- In JS, the `&&` is a control flow operator. It works like the if/else except it's an expression instead of a statement.
- Here is the same logic as above, but in an if/else.

```JAVASCRIPT
function Friend({ name, isOnline }) {
  let prefix;
  if (isOnline) {
    prefix = <div className="green-dot" />;
  } else {
    prefix = isOnline;
  }

  return (
    <li>
      {prefix}
      {name}
    </li>
  );
}
```

- The `&&` operator i sa "control flow" operator, like the if/else, it will always result in one of two paths taken.

- If the left hand value of isOnline `{isOnline && <div className="green-dot" />}` is false, the expression short-circuits, evaluates `isOnline` to `false.

- If the value it `true`, it evaluates tow whatever is on the right hand side of the operator `{isOnline && <div className="green-dot" />}`, which is the `<div>` element containing our green dot.

- 🎁 You can check out Logical Operators for JS primer. or [MDN Logical AND &&](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND)

##### Common galcha: the number zero

- The `&&` operator doesn't return `true` or `false`. It returns the left hand side or the right hand side.
- React will render any number you give it, even zero.
- React will ignore most falsy values, like `false` or `null`. But it won't ignore the number zero.

- See how React renders the different falsy value:
- React will actually render the zero.

```CODE
function App() {
  return (
    <ul>
      <li>false: {false}</li>
      <li>undefined: {undefined}</li>
      <li>null: {null}</li>
      <li>Empty string: {''}</li>
      <li>Zero: {0}</li>
      <li>NaN: {NaN}</li>
    </ul>
  );
}

export default App;
```

##### Solutions: Always use boolean values with the &&

- To avoid having random `0` in your app.
- Follow the golden rule: make sure the left-hand side of the `&&` always evaluates to a boolean value, either `true` or `false`

```JAVASCRIPT
function App() {
  const shoppingList = ['avocado', 'banana', 'cinnamon'];
  const numOfItems = shoppingList.length;

  return (
    <div>
      {numOfItems > 0 && (
        <ShoppingList items={shoppingList} />
      )}
    </div>
  );
}
```

- In this approach, we are specific with what the condition is: if we have 1 or more items int eh shopping list, we should render the `<ShoppingList>` element.
- The greater than operator `>` wil always produce a boolean value, `true` or `false`

- We can also convert any non-boolean value to a boolean value with `!!`

```JAVASCRIPT
function App() {
  const shoppingList = ['avocado', 'banana', 'cinnamon'];
  const numOfItems = shoppingList.length;

  return (
    <div>
      {!!numOfItems && (
        <ShoppingList items={shoppingList} />
      )}
    </div>
  );
}
```

- 👀 You can check out the JS primer on `!!`. Also on MDN, [Logical NOT operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_NOT)
- `!!` isn't actually a JS operator; we are repeating the NOT operator `!` twice.
- We can use the `!` to flip the boolean value:

```JAVASCRIPT
!true; // false
!!true; // true
```

- If we use the `!` with a non-boolean value, it will flip a truthy value to `false`, or a falsy value to `true`.

```JAVASCRIPT
4;

// true
!4; // false, since 4 is truthy

0; // falsy
!0; // true, since 0 is falsy
```

#### With Ternary

- What if you wanted to reproduce an `if`/`else` in our JSX?

- For example, building a use dashboard, if user is logged in, we want to render the charts. If not logged in, we want to render a short message asking them to log in.

- You could use two `&&` like so,

```JAVASCRIPT
function App({ user }) {
  const is LoggedIn = !!user;

  return (
    <>
      {isLoggedIn && <AdminDashboard />}
      {!isLoggedIn && <p>Please login first</p>}
    </>
  );
}
```

- This works, btu it's a bit clunky. In this case, we can use the **ternary operator**.
- 👀 MDN reference, [Conditional ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)

- Same code above, with ternary

```JAVASCRIPT
function App({ user }) {
  const isLoggedIn = !!user;

  return (
    <>
     {isLoggedIn
       ? <AdminDashboard />
       : <p>Please login first</p>
     }
    </>
  )
}
```

- Ternary operator has been part of JS since IE 3 in 1996!
- It rose to prominence with React, because it allows us to embed the if/else logic within out JSX. Because a ternary operator is an operator instead fo a statement, it can be used inside JS expressions.

- It consists of three parts:

```JAVASCRIPT
condition ? firstExpression : secondExpression;
```

If `condition` si truthy, the first expression will get evaluated. It falsy, the second expression wil be evaluated instead.

#### Showing and Hiding

- So far, we have seen a way to add or remove a chunk of markup on the page.
- Another way to solve this problem, toggle the visibility using CSS.

- An example;

```JAVASCRIPT
function Friend({ name, isOnline }) {
  const style = isOnline
    ? undefined
    : { display: 'none' };

  return (
    <li>
      <div
        className="green-dot"
        style={style}
      />
      {name}
    </li>
  );
}
```

- If the friend is online, `style` will be `undefined`, and wil have no effect. the green dot will be shown.

- If the friend is offline, the green dot will have `display: none` applied, effectively removing it form the UI.

- 📣 Inline styles? Using inline styles as an example; many ways to accomplish will explore later in the course.

#### Comparing approaches, Conditional render vs. CSS show/hide

- Interesting things on performance
- DOM nodes consume memory just by existing, no matter what, visible or not visible.
- In a large enough app, it can be beneficial to conditionally render to reduce the number of DOM nodes at any given time.
- The down side, adding DOM nodes can be a much slower task than toggling a CSS property.

- But in most cases the performance differences are so small, does not matter.

- Suggest: use conditional rendering by default, test on low end devices, is something seems slow, try an alternative approach.

#### Exercises

##### Supporting screen readers

- So far, we have used JS operators to conditionally render a green circle next to online users' names, but what happens when someone visits our app using a screen reader?

```JAVASCRIPT
<ul className="friends-list">
  <li className="friend">
    Andrew
  </li>
  <li className="friend">
    <div className="green-dot" />
    Beatrice
  </li>
  <li className="friend">
    <div className="green-dot" />
    Chen
  </li>
</ul>;
```

- The trouble is that `<div class="green-dot">` is semantically meaningless, so screen readers ignore it.

- How can we make this info available to folks who use a screen reader?
- Recommend using some CSS to visually hide a chunk of text.
- Using a custom `<VisuallyHidden>` component.

```JAVASCRIPT
<p>
  This text is shown normally.
  <VisuallyHidden>
    This text isn't on the screen, but is announced by screen readers.
  </VisuallyHidden>
</p>;
```

- This exercise, use the `VisuallyHidden` component to add the suffix "(Online)" after the names of online users.
- Check out [Josh's blog post Snippet](https://www.joshwcomeau.com/snippets/react-components/visually-hidden/)

- AC's
  - Users who are online should have the text "(Online)" added after their names.
  - The `VisuallyHidden` component should be used to make sure that this text isn't shown visually.
  - Users who are offline should not be affected.

```JAVASCRIPT
// starter code
import VisuallyHidden from './VisuallyHidden';

function Friend({ name, isOnline }) {
  return (
    <li className="friend">
      {isOnline && <div className="green-dot" />}
      {name}
    </li>
  );
}

function App() {
  return (
    <ul className="friend-list">
      <Friend name="Andrew" isOnline={false} />
      <Friend name="Beatrice" isOnline />
      <Friend name="Chen" isOnline />
    </ul>
  );
}

export default App;
```

- My attempt

```JAVASCRIPT
// my attempt
import VisuallyHidden from './VisuallyHidden';

function Friend({ name, isOnline }) {
  return (
    <li className="friend">
      {isOnline && <div className="green-dot" />}
      {name}
      {' '}
      {isOnline && <VisuallyHidden>(Online)</VisuallyHidden>}
    </li>
  );
}

function App() {
  return (
    <ul className="friend-list">
      <Friend name="Andrew" isOnline={false} />
      <Friend name="Beatrice" isOnline />
      <Friend name="Chen" isOnline />
    </ul>
  );
}

export default App;
```

- Added another expression slot after the name, with the logical `&&` operator, if `inOnline` is true, it will return the `VisuallyHidden` component.

```JAVASCRIPT
import VisuallyHidden from './VisuallyHidden';

function Friend({ name, isOnline }) {
  return (
    <li className="friend">
      {isOnline && <div className="green-dot" />}
      {name}
      {' '}
      {isOnline && <VisuallyHidden>(Online)</VisuallyHidden>}
    </li>
  );
}

function App() {
  return (
    <ul className="friend-list">
      <Friend name="Andrew" isOnline={false} />
      <Friend name="Beatrice" isOnline />
      <Friend name="Chen" isOnline />
    </ul>
  );
}

export default App;
```

- Here is the visually hidden component, css to hide visually but still read for screen readers.

```JAVASCRIPT
// Visually Hidden component
// These styles will make sure the component
// is not visible, but will still be announced
// by screen readers.
//
// Adding “display: none” would hide the
// element from ALL users, including those
// using screen-readers.
const hiddenStyles = {
  display: 'inline-block',
  position: 'absolute',
  overflow: 'hidden',
  clip: 'rect(0 0 0 0)',
  height: 1,
  width: 1,
  margin: -1,
  padding: 0,
  border: 0,
};

const VisuallyHidden = ({ children }) => (
  <span style={hiddenStyles}>
    {children}
  </span>
);

export default VisuallyHidden;
```

##### Exercises, User Profile with Badges

- Most social sites have community badges, awards for people who achieve certain goals.
- In this exercise, we want to update a set of user profiles to conditionally render some badges. For users who have no badges, the section will be omitted.

- The markup for the badges, looks like:

```HTML
<ul class="badge-list">
  <li>Badge 1</li>
  <li>Badge 2</li>
  <li>Badge 3</li>
</ul>
```

- AC's

1. If the user has at least 1 badge, an unordered list with the class `badge-list` should be rendered, using the data from `profile.badges`.
2. Each badge should be its own list item, with the `badges.label` being rendered within.
3. There should be no "key" warnings in the browser console. You can trust that the badge slugs are unique.

- Stretch goal:

4. If the user has 3 or more badges, a golden color should be applied:
5. This can be done, by adding the `golden` class to the `<ul>`:

```JAVASCRIPT
// STARTER CODE
// GOAL:
// Render an unordered list with the class
// “badge-list” when the user has at least
// 1 badge.
//
// Each badge is an object with this shape:
// { slug: string, label: string }
//
// STRETCH:
// If the user has 3+ badges, the “golden”
// class should be added to the unordered
// list (in addition to “badge-list”).

function ProfileCard({ profile }) {
  return (
    <article className="profile-card">
      <header>
        <img
          alt={profile.imageAlt}
          src={profile.imageSrc}
        />

        <h2>{profile.name}</h2>
        <p className="joined">
          Joined
          {' '}
          {profile.joinDate}
        </p>
      </header>

    </article>
  );
}

export default ProfileCard;
```

- TIP: Use `map()` to iterate through the `<li>`

```JAVASCRIPT
{ profile.badges.map(({ slug, label }) => (
  <li key={slug}>{label}</li>
)); }
```

- Solution:

```JAVASCRIPT
// “badge-list” when the user has at least
// 1 badge.
//
// Each badge is an object with this shape:
// { slug: string, label: string }
//
// STRETCH:
// If the user has 3+ badges, the “golden”
// class should be added to the unordered
// list (in addition to “badge-list”).

function ProfileCard({ profile }) {
  const badges = (
    // TERNARY TO RENDER THE golden class
    <ul className={profile.badges.length >= 3
      ? 'badge-list golden'
      : 'badge-list'
      }
    >
      {profile.badges.map(({ slug, label }) => (
        <li key={slug}>{label}</li>
      ))}
    </ul>
  );

  return (
    <article className="profile-card">
      <header>
        <img
          alt={profile.imageAlt}
          src={profile.imageSrc}
        />

        <h2>{profile.name}</h2>
        <p className="joined">
          Joined
          {' '}
          {profile.joinDate}
        </p>
        {profile.badges.length > 0 && badges}
      </header>

    </article>
  );
}

export default ProfileCard;
```

#### Range Utility

- A common use case, you want to render a rating system with 5-stars. You want to render 0 to 5 little star icons, depending on the rating.

- In JS, our go to is the `for` loop. But the `for` loop is a statement and we cannot use statements in our JSX.

- Here is one solution, we can use a `for` loop above the JSX, to create our array of elements.
- MDN `for` [refresher](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)

```JAVASCRIPT
function StarRating({ rating }) {
  /*
    Here's the markup for a single star:

    <img
      alt=""
      className="gold-star"
      src="https://sandpack-bundler.vercel.app/img/gold-star.svg"
    />

    Your job is to repeat this element
    based on the `rating` prop.
    If the rating is 4, we need 4 copies.
  */

  const stars = [];

  for (let i = 0; i < rating; i++) {
    stars.push(
      <img
        key={i}
        alt=""
        className="gold-star"
        src="https://sandpack-bundler.vercel.app/img/gold-star.svg"
      />,
    );
  }

  return (
    <div className="star-wrapper">
      {stars}
    </div>
  );
}

export default StarRating;
```

- We create an array of image elements with a `for` loop, and then we render that array inside oru JSX.
- Similar to the `.map`, React can unpack arrays for us, and render each of the elements inside, so long as we provide a unique `key` prop.

##### A functional alternative

- An alternative approach is the `range` function.
- `range` is a utility function. It is not part of the JS language. but it is in utility libraries like [lodash](https://lodash.com/).

- Examples to create an array

```JAVASCRIPT
// Create an array from 0 (inclusive) to 2 (exclusive):
range(2);
// Produces: [0, 1]

// Create an array from 0 (inclusive) to 5 (exclusive):
range(5);
// Produces: [0, 1, 2, 3, 4]

// Create an array from 2 (inclusive) to 6 (exclusive):
range(2, 6);
// Produces: [2, 3, 4, 5]

// Create an array from 2 to 10, picking every 2nd number
range(2, 10, 2);
// Produces: [2, 4, 6, 8]
```

- It's basically an expression version of a `for` loop statement, like how `&&` can be an expression version of an `if` statement. So we can use it in our JSX.

- How we use it for the star rating:

```JAVASCRIPT
function StarRating({ rating }) {
  return (
    <div className="star-wrapper">
      {range(rating).map(num => (
        <img
          key={num}
          alt=""
          className="gold-star"
          src="https://sandpack-bundler.vercel.app/img/gold-star.svg"
        />
      ))}
    </div>
  );
}
```

- `range(rating)` will produce an array from 0 to `n`, where `n` is the supplied rating.
- Then use the `.map` trick to iterate over that array, creating a copy of our star image for each one.
- For the `key` prop, we use the number generated within teh array, since we know it's unique.

##### Range function code

- Code for the `range` function.
- You can either include this in your `utils.js` or load from [lodash](https://lodash.com/docs/4.17.15#range).

```JAVASCRIPT
const range = (start, end, step = 1) => {
  const output = [];

  if (typeof end === 'undefined') {
    end = start;
    start = 0;
  }

  for (let i = start; i < end; i += step) {
    output.push(i);
  }

  return output;
};
```

#### Exercises, Range Utility

- Rendering a grid
- Suppose we are building a Scrabble like word game, and we want to render a grid of HTML elements.
- Here is the DOM structure for a 2.3 grid:

```HTML
<div class="grid">
  <div class="row">
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
  </div>
  <div class="row">
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
  </div>
</div>
```

- Your mission should you choose it, it to replicate this structure, but for a variable number of rows and columns.

- AC's
- 1. Use the template for a `Grid` component, which will be provided with a `numRows` prop for the number of rows, and a `numCols` prop for the number of columns.
- 2. There should be X divs with a class of `row`, where X is equal to the `numRows` prop.
- 3. Inside each `row`, there should be Y divs with a class of `cell`, where Y is equal to the `numCols` prop.
- 4. You should use the provided `range` function to solve this problem.
- 5. There shouldn't be any key-related warnings in the console. You an assume that the grid is "static", with no plans to support reordering the rows or cells.

- Starter code:

```JAVASCRIPT
// starter code
import { range } from './utils';

function Grid({ numRows, numCols }) {
  return (
    <div className="grid">
      {/* TODO */}
    </div>
  );
}

export default Grid;
```

- My attempt:

```JAVASCRIPT
// my attempt
import { range } from './utils';

function Grid({ numRows, numCols }) {
  return (
    <div className="grid">
      {
        range(numRows).map(num => (
          <div key={num} className="row">
            {range(numCols).map(num => (
              <div key={num} className="cell">👩‍🚀</div>
            ))}
          </div>
        ))
      }
    </div>
  );
}

export default Grid;
```

- 1. Use `range()` and `map()` to iterate over the rows. Passing in the `numRows`

```JAVASCRIPT
  return (
    <div className="grid">
      {
        range(numRows).map(num => (
          <div key={num} className="row">
              <div class="cell"></div>
          </div>
        ))
      }
    </div>
  );
```

- 2. Then do the same thing for the columns.

```JAVASCRIPT
  {range(numCols).map(num => (
    <div key={num} className="cell">👩‍🚀</div>
  ))}
```

- 🤔 Keys, in this case the are nested keys, within the scope of each array.
