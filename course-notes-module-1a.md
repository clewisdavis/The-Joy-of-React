# Module 1 - Understanding JSX

- [Course Outline Notes](course-notes.md)

## Understanding JSX

- In the previous example, we used everyday JS.
- In reality, most engineers use a syntax called JSX
- For example:

```JAVASCRIPT
// // Old way:
// const element = React.createElement(
//   'p',
//   {
//     id: 'hello',
//   },
//   'Hello World!'
// );

// New way:
const element = (
  <p id="hello">
    Hello World!
  </p>
);
```

- Instead of writing `Rect.createElement`, we use an HTML like syntax to create React elements.

- Why use JSX? As your codebase grows, it becomes easier to read with JSX.
- React elements can form a tree structure, just like HTML. This happens when we set the "children" parameter of a React element to another React element.

- Written out, we wind up with a really long tree structures that are hard to read using plain JS:

```JAVASCRIPT
// Plain JS:
const element = React.createElement(
  'nav',
  { id: 'main-nav' },
  React.createElement(
    'ul',
    null,
    React.createElement(
      'li',
      null,
      React.createElement(
        'a',
        { href: '/' },
        'Home',
      ),
    ),
    React.createElement(
      'li',
      null,
      React.createElement(
        'a',
        { href: '/archives' },
        'Archives',
      ),
    ),
  ),
);
```

- Now compare that same markup in JSX, much easier to read.

```HTML
// In JSX:
const element = (
  <nav id="main-nav">
    <ul>
      <li>
        <a href="/">
          Home
        </a>
      </li>
      <li>
        <a href="/archives">
          Archives
        </a>
      </li>
    </ul>
  </nav>
);
```

- ℹ️ Why the parenthesis around the JSX? This is for formatting purposes. Allows us to push the JSX onto hte next line. Even outside of JSX/React, Parentheses can be used to improve formatting.

### Compiling JSX into JS

- If you try and run the JSX code int he browser, you will get an error.
- JS engines don't understand JSX, they only under stand JS.
- We need to compile our code into plain JS.

- This is done in a build step, tools like Babel.

- ℹ️ The JSX we write gets converted into `React.createElement`. By the time our code is running int he user's browser, al the JSX has been zapped out, and we are left with a JS file, full of nested `React.createElement` calls.

- React import? Best practice to always import React, `import React from 'react';`
- In JSX, it's obfuscated, meaning it's not used by JSX, only after JSX becomes compiled into `React.createElement`

### Expression Slots

- When you want to see the result of an expression, you have to wrap it in a `{}`
- But what is actually going on here?

```JAVASCRIPT
import { render } from 'react-dom';

const shoppingList = ['apple', 'banana', 'carrot'];

const element = (
  <div>
      Items left to purchase: shoppingList.length
  </div>
);

const root = document.querySelector('#root');

render(element, root);
```

- In this example, shoppingList.length is unprocessed, just renders as a string.
- You can fix this by wrapping it in a `{}`, and it processed it as JS, instead fo a static sting.
- What are the rules for this?
  - React is doing very little here, it's the rules on the JS side you have to pay attention to.

- The pure JS version of this is.

```JAVASCRIPT
const compiledElement = React.createElement(
  'div',
  {},
  'Items left to purchase: shoppingList.length',
);
```

- When you add the `{}`, you create an EXPRESSION SLOT
- This is a slot we can put any JS expression in, and it will be forwarded along by React.

```JAVASCRIPT
const element = (
  <div>
      Items left to purchase:
    {shoppingList.length}
  </div>
);

const compiledElement = React.createElement(
  'div',
  {},
  'Items left to purchase: ',
  shoppingList.length,
);
```

- All React is doing, whatever is in the `{}` gets forwarded along, and when React compiles the JSX into pure JS.
- This means we are subject to all the rules of JS.

- Have to understand the difference between statements vs. expressions.
- For example, you cannot just put an `if()` statement.
- Only allowed to use expressions within our JSX.
- You can put any valid expression in the `{}`, for example; `{Math.random()}`

- You must use `expressions`, because you cannot use `statements` in the middle of a function call.

### Comments in JSX

- To add a comment in JSX, we use expression slot:

```JAVASCRIPT
const element = (
    <div>
     {/* some comment */}
    <div>
);
```

- Single line syntax comments everything out, including the `{}` for the expression slot.

### Attribute expression slots

- We can use the same trick for dynamic attribute values.

```JAVASCRIPT
const someIdentifier = 'some-unique-identifier';

const element = (
  <div id={someIdentifier}>
      Hello World
  </div>
);
```

- This allow su to create expression slots for the value of the `id` attribute.
- How it compiles.

```JAVASCRIPT
const element = React.createElement(
  'div',
  {
    id: someIdentifier,
  },
  'Hello World',
);
```

- Only two ways to use expression slots, to dynamically calculate children, or attributes.

### Type coercion

- React wil automatically convert types as required when supplying attributes in JSX.
- These are identical.

```JAVASCRIPT
// This works:
<input required="true" />

// And so does this
<input required={true} />
```

- In the first example, we set the required attribute as a string `"true"`, second example, it's set as a boolean attribute `true`. Either way, the input will wind up being required.

- You can also pass numbers or strings for number attribute:

```JAVASCRIPT
// ✅ Valid
<input type="range" min="1" max="20" />
// ✅ Valid
<input type="range" min={1} max={20} />
```

- ℹ️ Boolean attributes:
- In HTML, it's possible to set attributes to `true` just by specifying only the key.
- `<input required>`
- In JSX, these are equivalent:
- `<input required />` and `<input required={true} />`
- Prefer to spell it out.

### Differences from HTML

- JSX looks like HTML, but are some fundamental differences.

- **Reserved Words**
- In JS, has a couple of reserved words, keywords with built in functionality and we cannot use them as variable names.

- For example, `const while = 10;`, if we run this code, we get a syntax error, `while` is reserved for "while loops".

- Becurse JSX gets transformed into JS, cannot use reserved word in our JSX. This can be a problem, sometimes, HTML attributes sometimes overlap with oru JS reserved words.

- Example; `for` and `class`
- React uses a slight variation on these two terms: `htmlForm` and `className`

```JAVASCRIPT
const element = (
  <div>
    <label htmlFor="name">
      Name:
    </label>
    <input
      id="name"
      className="fun-input"
    />
  </div>
);
```

### Self closing Tags

- HTML is a pretty forgiving language when it comes to closing tags. For example, this is valid HTML

```HTML
<div>
  <p>This paragraph is opened… but never closed.
  <p>We're omitting the closing tags!
</div>
```

- The browser is smart enough to know, to close the paragraph tag.
- But for JSX, we need to close every tag we open.

```JAVASCRIPT
const element = (
  <div>
    <p>This is a paragraph</p>
  </div>
);
```

- In HTML5, some elements don't have closing tags, the `img` tag for example.

```HTML
<img alt="A friendly dog" src="/img/dog.jpg">
```

- JSX won't like this, need to close with a "self closing" tag.

```JAVASCRIPT
const element = (
  <img
    alt="A friendly kitten"
    src="/images/kitten.jpg"
  />
);
```

### Case sensitive tags

- HTML is a case insensitive language. Many years ago, it was common to write HTML in all uppercase.

```HTML
<MAIN>
  <HEADER>
    <H1>Hello World!</H1>
  </HEADER>
  <P>
    This HTML is so loud!
  </P>
</MAIN>
```

- JSX, is case sensitive, our gags must all be lowercase.

```JAVASCRIPT
const element = (
  <main>
    <header>
      <h1>Hello World!</h1>
    </header>
    <p>
      This HTML is so loud!
    </p>
  </main>
);
```

- Good reason for this, JSX compiler uses the tag's case to tell whether it's a primitive part of the DOM or a custom component.

### Case Sensitive attributes

- In JSX, our attributes need to be "camelCase"
- For example, this is valid HTML

```HTML
<video
  src="/videos/cat-skateboarding.mp4"
  autoplay={true}
>
```

- In JSX, we need to capitalize the 'p' in 'autoplay'.

```JAVASCRIPT
const element = (
  <video
    src="/videos/cat-skateboarding.mp4"
    autoPlay
  />
);
```

- Other properties that need to be 'camelCased' include:

- `onclick` to `onClick`
- `tabindex` to `tabIndex`
- `stroke-dasharray` to `strokeDasharray` (specific to SVG)

- ℹ️ Data and ARIA attributes keep their dashes.
- Two exceptions to the 'camelCasing' of attributes: data attributes and ARIA attributes.
- For example, this is valid JSX

```JAVASCRIPT
const element = (
  <button
    data-test-id="close-dialog-button"
    aria-label="Close dialog"
  >
    <img alt="" src="/icons/x.svg" />
  </button>
);
```

### Inline Styles

- In HTML, teh style attribute allows ut o apply some styles inline, to an element.

```HTML
<h1 style="font-size: 2rem;">
  Hello World!
</h1>
```

- In JSX, the `style` instead takes an object:

```JAVASCRIPT
const element = (
  <h1 style={{ fontSize: '2rem' }}>
    Hello World!
  </h1>
);
```

- ℹ️ Why the two squiggly brackets?
- 1. The outer set is for the "expression slot" in the JSX, to hold the JS expression.
- 2. The inner set creates a JS object.

- All CS properties are written in camelCase, every sash is replaced by capitalizing the subsequent word:

- `background-position` becomes `backgroundPosition`
- `border-bottom-color` becomes `borderBottomColor`
- `-webkit-font-smoothing` becomes `WebkitFontSmoothing`

- And, React wil automatically apply the `px` suffix for certain properties.

```JAVASCRIPT
<div
  style={{
    width: 200, // Equivalent to `width: 200px`
    paddingTop: 8, // Equivalent to `padding-top: 8px`
  }}
>
```

- Watch out for unitless properties like `flex` or `lineHeight`
- In Rect, you can go ahead and use the unit if you prefer.

```JAVASCRIPT
<p
  style={{
    width: '200px',
    paddingTop: '8px',
  }}
>
```

### Exercise: Fix the Converter

- Fun and games, a quiz to spot the errors.
- Cool online HTML to JSX converter. <https://transform.tools/html-to-jsx>

### The Whitespace Gotcha

- One of the most common "gotchas" with JSX.

```JAVASCRIPT
import { createRoot } from 'react-dom/client';

const daysUntilSantaReturns = 123;

const element = (
  <div>
    <strong>
      Days until Santa returns:
    </strong>
    {daysUntilSantaReturns}
  </div>
);

const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

- In this example, no space between the bold text and the number. It renders `returns:123`.
- Why, consider how this compiles to JS.

```JAVASCRIPT
const element = React.createElement(
  'div',
  {},
  React.createElement(
    'strong',
    {},
    'Days until Santa returns:',
  ),
  daysUntilSantaReturns,
);
```

- Our div has two children, the `<strong>` tag and the `daysUntilSantaReturns` variable.
- JSX doesn't compile to HTML, it compiles to JS. And when that JS is executed, it's only going to create and append two HTML nodes.

  - A `<strong>` tag with some text
  - A text node, for the number `123`

- So how do we fix it? The most common solution is to add a single whitespace character in curly braces:

```JAVASCRIPT
<div>
  <strong>Days until Santa returns:</strong>
  {' '}
  {daysUntilSantaReturns}
</div>;
```

- And it compiles like this:

```JAVASCRIPT
const element = React.createElement(
  'div',
  {},
  React.createElement(
    'strong',
    {},
    'Days until Santa returns:',
  ),
  ' ', // explicit space character
  daysUntilSantaReturns,
);
```

- This is a feature in React, to tell React, this is meant to be a space character `{' '}`
- ℹ️ Tools of the Trade: Prettier is used to automatically format oru code. It knows about the white space issue and will automatically add the `{' '}` character when necessary. <https://courses.joshwcomeau.com/joy-of-react/11-tools-of-the-trade/04-prettier>

### Exercises

#### Build a Search Form

- In this exercise, we will build an online search form, inline search form.
- All style are provided, you just need to add three elements inside the `<form>` tag:

1. A label
2. A text input
3. A button, with teh class `submit-btn`

- Acceptance Criteria:
  - The UI should match the screenshot above
  - The label should be attached to the input; clicking the label text should focus the input
  - The button should have the appropriate styles, applied with the class `submit-btn`

```JAVASCRIPT
// index.js
import React from 'react';
import { createRoot } from 'react-dom/client';

const element = (
  <form>
    <label htmlFor="search-input">Search:</label>
    <input id="search-input" type="text" />
    <button className="submit-btn" type="button" aria-label="Search">Submit</button>
    {/* Stuff here */}
  </form>
);

const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

- Need to write JSX, `for` and `class` are reserved JS, to convert those to JSX, `htmlFor` and `className`

🚀 Check out Josh W's custom reset. <https://www.joshwcomeau.com/css/custom-css-reset/>

**Critter**

- Build a Twitter/Animal hybrid, a social media app for animals.

- Already given two things:
  - A `message` object that contains all the data you need to populate the UI
  - All the styles, so we don't have to worry about any of the CSS, need to follow the semantic markup.

- Acceptance Criteria:

  - The UI should match the mockup, using the data from the `message` object
  - The user's avatar should have helpful descriptive alt text
  - The user's name should be a link, and it should link to `/users/[username]`. With this data, it should be `/users/benjamintorn`. No actual profile page, so link will not do anything
  - The footer should include the word "Posted" before the published date.

- HINT: You might want to start with the message content and footer, saving the header for last.

- ℹ️ Use string interpolation for the url, `${}` is how you do string interpolation. So for the url, you can do.

- Solution Code:

```JAVASCRIPT
import React from 'react';
import { createRoot } from 'react-dom/client';

const message = {
  content:
    'Just ate at “Les Bordeaux En Colère”. Wonderful little venue!',
  published: 'January 21st at 9:45pm',
  author: {
    avatarSrc: 'https://sandpack-bundler.vercel.app/img/avatars/009.png',
    avatarDescription: 'Cartoon bear',
    name: 'Ben Thorn',
    handle: 'benjaminthorn',
  },
};

const profileUrl = `/users/${message.author.handle}`;
const imageAlt = `${message.author.avatarDescription} (user profile photo)`;

const element = (
  <article>
    <header>
      <img
        alt={imageAlt}
        src={message.author.avatarSrc}
      />
      <a href={profileUrl}>{message.author.name}</a>
    </header>
    <p>{message.content}</p>
    <footer>
Posted
      {' '}
      {message.published}
    </footer>
  </article>
);

const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

- String interpolation:

```JAVASCRIPT
<a href={`/users/${message.author.handle}`}>
  {message.author.name}
</a>;
```

- To make this easier to read, you can just put it into a variable.

```JAVASCRIPT
const profileUrl = `/users/${message.author.handle}`;
```

- Then you can just use that variable. Check out the lesson on string interpolation with template strings.
- <https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/03-string-interpolation>

- Modern JS, allows ust o embed variables and other expressions right inside strings:

```JAVASCRIPT
const dynamicString = `hello ${userName}!`;
```

- In order rto use string interpolation, we need to use the backticks (`).
- Strings created with backtics are known as "template strings". With one super-power, can embed dynamic segments.

- We create a dynamic segment within our string, by writing `${}`.
- Anything placed between that squiggly bracket wil be evaluated as a JS expression.

- For example; we can do things like.

```JAVASCRIPT
const age = 7;
const className = `Next year,  you'll be ${age + 1} years old.`;
console.log(className);
// Next year, you'll be 8 years old.
```

- Write that in the browser console, and you will see.

### JSX vs. Templates

- JSX compared to template languages like Handlebars or EJS or Pug.
- Optional lesson for a deep dive.
