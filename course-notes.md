# The Joy of React

Course notes from clewisdavis, for future reference.

- Start date February 2023

## Module 1

- The magic of React, you write code that describes what the UI should be based on. React will figure out what's changed and update the UI.
- History of React and how it got started at [FB](https://www.youtube.com/watch?v=nYkdrAPrdcw&t=624s).

### Hello React

- Start with a hello world React App, using pure JS.

1. Import dependencies

```JAVASCRIPT
import React from 'react';
import { createRoot } from 'react-dom/client';
```

- Import the core React library from the `react` dependency and the `createRoot` function from the `react-dom`
- React is platform agnostic, you have the core `react` package, and then you have platform-specific renders:
  - `react-dom` for the web
  - `react-native` for mobile (iOS / Android) or desktop (Windows / MacOS) apps
  - `react-three-fiber` for 3D scenes using WebGL and Three.js
- All the platforms use the same core framework, `react`, but you have to specify the correct package based on what you are building.

- You can use the React skills you learn, to build apps across platforms.

- What is the DOM?
  - It's an HTML blueprint of a house, and the DOM is the house itself.
  - Rect, works by interacting with teh DOM via JS. To create, update, and delete DOM elements as required.
  - Document Object Model.

2. Create a React element

- With the following code.

```JAVASCRIPT
const element = React.createElement(
  'p',
  { id: 'hello' },
  'Hello World!',
);
```

- The `React.createElement()` is a function that accepts 3 or more arguments.
    1. The type of element
    2. The properties we want this element to have
    3. The element's contents, what the element should have as children.

- This function returns a React element. Plain old JS objects, if you console log it, you will see it's a description of a paragraph tg.

- DOM hierarchy, is organized like a tree, elements have parents, sibling and children.
- React elements form a hierarchy just like the DOM elements do.

3. Render the application

- The code to mount and render the application

```JAVASCRIPT
const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

- The `document.querySelector` is a reference point of the existing DOM element.
- In your `index.html` file, you include the `id="root"` element to the DOM element you want to mount the app. `<div id="root"></div>`

- This element wil be the apps container.
- This element will be the root of our app.

- And the render the application with `root.render(element)`.
- The `render` function converts Rect elements into DOM nodes.
- In this case, react describes a paragraph tag, an dID and some text.
- `render` will turn that description into the following DOM element.

```HTML
<p id="hello">
  Hello World!
</p>
```

- With that DOM element created, it adds it to the page at the specified root.
- **It takes a JS based description of some HTML, and uses it to produce real-world DOM nodes.**

- But, the real magic happens when things change.
- NOTE: React 17, used an different API syntax from the example above.

## Build your own React

- Build your own `render` function that takes React elements and produces the equivalent DOM structure.

- Write a `render` function that accepts a Rect element and a reference to a DOM container element that will hold our app.

- Acceptance criteria:
  - A link should be shown in the "result" pane, linking to a wikipedia.org, adn with the text "Read more on Wikipedia"
  - It should work with any element type (eg anchors, paragraphs, button...)
  - It should handle all HTML attributes (eg. href, id, disabled...)
  - The element should contain the text specified under `children`. `children` will always be a stirring.

```JAVASCRIPT
// SOLUTION
function render(reactElement, containerDOMElement) {
  // 1. create a DOM element
  const domElement = document.createElement(reactElement.type);

  // 2. update properties
  domElement.innerText = reactElement.children;
  for (const key in reactElement.props) {
    const value = reactElement.props[key];
    domElement.setAttribute(key, value);
  }

  // 3. put it in the container
  containerDOMElement.appendChild(domElement);
}
```

- Boils down to, React takes an JS object, and describes how to build the DOM elements.
- React takes those descriptions, and creates the real DOM.  
- React-DOM, is the translator, takes the descriptions we product in JS, and builds, produces the real version of them.

- Remember, it's a JS framework, and fundamentally uses the same things you would use, if building in JS.

### Productive failure

- A learning method used from academic.
- n Productive Failure, the conventional instruction process is reversed so that learners attempt to solve challenging problems ahead of receiving explicit instruction. While students often fail to produce satisfactory solutions (hence “Failure”), these attempts help learners encode key features and learn better from subsequent instruction (hence “Productive”).
- Much more likely to remember things, if you fail first, not given all the instructions ahead of time.
- Struggling means you are learning, and growing.

- The growth mindset, we become smarter through practice, and failure is the fastest way to learn.

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

**Build a Search Form**

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

## Components

- Components are a huge part of React, one thing to know, it's a component based framework.
- What is a component exactly? **A component is a bundle of markup, styles, and logic that controls everything about a specific part of the user interface.**
- It's a different mental model when it comes to code organization. Instead of seperating our apps into markup (HTML), styles (CSS), and logic (JS), we organize our apps into components.
- Good image reference: <https://twitter.com/areaweb>

### Mechanisms of reuse

- Traditional HTML doesn't really have a way to reuse markup. Some languages use `partials` to achieve this. A chunk of HTML inserted into another HTML document.

- In CSS, the main way to reuse is a `class`. A `btn` style for example.

```CSS
.btn {
  padding: 8px 32px;
  background: blue;
  border: none;
  font-size: 1rem;
}
```

- For JS, the mechanism to reuse is the `function`. For example, a function to process data in some way:

```JAVASCRIPT
function shout(sentence) {
  return `${sentence.toUpperCase()}!!`;
}

shout('here we go');
```

- With React, components re the main mechanisms of reuse.
- Instead of partials for HTML, classes for CSS and function for JS, we create a component that bundles up all 3, and allows us to create ea library of high-level UI elements.

- ℹ️ MOdern React also features `hooks`, which offers a way to reuse React logic. In future lessons.

### Thinking in Components

- Go through the exercises and start thinking in terms of components.
- Annotate the screenshots, how would you organize them and break them down as components?
- Where would you re-use? How would you break them down.
- What can variants of the same component? One component with different instances?
- Is the difference enough to create a separate component? Button, vs Button Icons for example.
- Think in terms of broad strokes, and the page routes / navigation.
- Then start to break down the components within each route.

### Basic Syntax

- In React, components can be defined as JS functions.
- Typically, React components return one or more React elements.
- For example;

```JAVASCRIPT
import React from 'react';
import { createRoot } from 'react-dom/client';

function FriendlyGreeting() {
  return (
    <p
      style={{
        fontSize: '1.25rem',
        textAlign: 'center',
        color: 'sienna',
      }}
    >
      Greetings, weary traveler!
    </p>
  );
}

const container = document.querySelector('#root');
const root = createRoot(container);
root.render(<FriendlyGreeting />);
```

- This example. `FriendlyGreeting` create a React element that describes a paragraph, with some built in styles.
- We render a component just like hwo we render an HTML tag. Instead of rendering a `<div>` or a `<h1>`, we render a `<FriendlyGreeting>`.

- **The big component rule**

- One must rule to follow when creating components. React components need to start with a **Capital Letter**.
- Has to do with the JSX to JS transformation.
- If our component had a lower-case function name, React would render `<friendlygreeting>` HTML element, instead of processing it as a component.

- **A React element is a description of a thing we want to create.**
- In some cases we want to create a DOM node, like a `<p>`. In other cases, we want to create a component instance.

### Props

- So far, our `<FriendlyGreeting />` is kinda neat, but not that useful. It renders the same thing every time. Not that flexible.

- Components use `props`. They are like arguments to a function: allow you to pass data to our components, so the components can include customizations base on teh data.

- How would you tweak our greeting component to take a person's name?
- How do you funnel data into a component. Similar to giving data to and HTML element `<div id="some-div">Hello</div>`, passing `some-div` to that HTML element.
- You do the same thing, when it comes to React components, `<FriendlyGreeting name="chris" />`
- So what React does, as it's rendering, it collects all the data you specified, and calling the Component with that data.
- It does this through the props, which becomes an object.

```JAVASCRIPT
// Props is an object { name: "Josh" }
function FriendlyGreeting(props) {
  return (
    <p
      style={{
        fontSize: '1.25rem',
        textAlign: 'center',
        color: 'sienna',
      }}
    >
      Greetings, {props.name}!
    </p>
  );
}

render(
    <div>
      <FriendlyGreeting name="Chris" />
    </div>
    document.querySelector('#root');
);
```

- Now you can create the same component, but you can specify the data.
- ℹ️ Destructuring: To extract data directly from an object, directly in the function declaration, or anywhere else.

```JAVASCRIPT
// Destruction the props object, { name }
function FriendlyGreeting({ name }) {
  return (
    <p
      style={{
        fontSize: '1.25rem',
        textAlign: 'center',
        color: 'sienna',
      }}
    >
      Greetings, {name}!
    </p>
  );
}

render(
    <div>
      <FriendlyGreeting name="Oatie" />
      <FriendlyGreeting name="Chris" />
      <FriendlyGreeting name="Gibby" />
    </div>
    document.querySelector('#root');
);
```

- Helpful, because you can see all the different data in the function, don't have to go hunting. When you have a lot of data types. Example;

```JAVASCRIPT
function FriendlyGreeting({ name, age, location, className }) {
    return (
        // stuff here
    )
}
```

- For more info on [Object Destructuring](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/05-object-destructuring).

### Default values

- Let's suppose we are working on our `FriendlyGreeting` component. But we don't know everyone's name.

- For example, you can render a fallback value. I you know their name: `Hey Chris!`, If not, `Hey There!`

- We can do this with the `||` operator, like.

```JAVASCRIPT
function FriendlyGreeting({ name }) {
    return (
        <p>
          Hey {name || 'there'}!
        </p>
    );
}
```

- If `name` is provided, it will be used. Otherwise, we fall back to use "there".
- This works, but a better way to do it. Specify default values for each prop:

```JAVASCRIPT
function FriendlyGreeting({ name = 'there' }) {
  return (
    <p>
          Hey
      {' '}
      {name}
    </p>
  );
}
```

- Benefits, if you have multiple props with default values, we can see all the defaults int he same place.
- Occasionally the `||` operator will surprise us by using the default value even if we supplied a value.

- It's become a well-established convention to specify the default values within the prop object.

- Another example, we have a decorative "Horizontal Rule" component, essentially a way to draw a line between sections. Has a default width of 100px, btu we can override that value.

```JAVASCRIPT
function HorizontalRule({ width = 100 }) {
  return (
    <div style={{ width }}>
      {/* stuff here*/}
    </div>
  );
}

<HorizontalRule width={250} /> // 250px
<HorizontalRule />
```

- ℹ️ `defaultProps` property, is used by older versions of react. Don't use this property, the recommended approach is to use destructuring assignment.

### The Children Props

- Imagine you are building a custom button component. It should look and act just like a regular HTML button, but have red background and white text.

```JAVASCRIPT
function RedButton({ contents }) {
  return (
    <button
      style={{
        color: 'white',
        backgroundColor: 'red',
      }}
    >
      { contents }
    </button>
  );
}
```

- And we could use it like this:

```JAVASCRIPT
root.render(
  <RedButton contents="Click Me!" />,
);
```

- It works but not quite right, how we use a typical HTML button. When the content goes in-between the open and close tags.

```HTML
<button>
    Click Me!
</button>
```

- React allows us to do the same thing with oru custom components:

```JAVASCRIPT
root.render(
  <RedButton>
     Click Me!
  </RedButton>,
);
```

- When we do this, we can access the children through the `children` prop:

```JAVASCRIPT
function RedButton({ children }) {
  return (
    <button
      style={{
        color: 'white',
        backgroundColor: 'red',
      }}
    >
      {children}
    </button>
  );
}
```

- React does this for us, when we pass something between the open and close tags, react will automatically supply that value to use under `children`

- For example, if you console.log this element, you will see `children` under `props`

```JAVASCRIPT
import React from 'react';

const element = (
  <div>
     Hello World
  </div>
);

console.log(element);
```

- The result would be:

```JAVASCRIPT
{
  "type": "div",
  "key": null,
  "ref": null,
  "props": {
    "children": "Hello World"
  },
  "_owner": null,
  "_store": {}
}
```

- `children` is a special value, a "reserved word" when it comes to `props`
- However, it's not different from other props. It's the same.

### Exercises, Components

- Practice making some components, take the JSX and refactor the code so that it uses a component.

#### Building a CRM

- Written markup to display the contact info for 3 contacts, but way to much repetition involved. Create a new component `ContactCard` and use that for each of the 3 contacts.

```JAVASCRIPT
function App() {
  return (
    <ul>
      <li className="contact-card">
        <h2>Sunita Kumar</h2>
        <dl>
          <dt>Job</dt>
          <dd>Electrical Engineer</dd>
          <dt>Email</dt>
          <dd>sunita.kumar@acme.co</dd>
        </dl>
      </li>
      <li className="contact-card">
        <h2>Henderson G. Sterling II</h2>
        <dl>
          <dt>Job</dt>
          <dd>Receptionist</dd>
          <dt>Email</dt>
          <dd>henderson-the-second@acme.co</dd>
        </dl>
      </li>
      <li className="contact-card">
        <h2>Aoi Kobayashi</h2>
        <dl>
          <dt>Job</dt>
          <dd>President</dd>
          <dt>Email</dt>
          <dd>kobayashi.aoi@acme.co</dd>
        </dl>
      </li>
    </ul>
  );
}

export default App;
```

- My results

```JAVASCRIPT
function ContactCard(props) {
  return (
    <li className="contact-card">
      <h2>{props.name}</h2>
      <dl>
        <dt>Job</dt>
        <dd>{props.job}</dd>
        <dt>Email</dt>
        <dd>{props.email}</dd>
      </dl>
    </li>
  );
}

function App() {
  return (
    <ul>
      <ContactCard
        name="Chris Davis"
        job="UX Designer"
        email="chris.smith@acme.co"
      />
      <ContactCard
        name="Sunita Kumar"
        job="Electrical Engineer"
        email="sunita.kumar@acme.co"
      />
      <ContactCard
        name="Henderson G. Sterling II"
        job="Receptionist"
        email="henderson-the-second@acme.co"
      />
      <ContactCard
        name="Aoi Kobayashi"
        job="President"
        email="kobayashi.aoi@acme.co"
      />
    </ul>
  );
}
```

- To use destructuring to pass in the props, much cleaner.

```JAVASCRIPT
function ContactCard({ name, job, email }) {
  return (
    <li className="contact-card">
      <h2>{name}</h2>
      <dl>
        <dt>Job</dt>
        <dd>{job}</dd>
        <dt>Email</dt>
        <dd>{email}</dd>
      </dl>
    </li>
  );
}

function App() {
  return (
    <ul>
      <ContactCard
        name="Chris Davis"
        job="UX Designer"
        email="chris.smith@acme.co"
      />
      <ContactCard
        name="Sunita Kumar"
        job="Electrical Engineer"
        email="sunita.kumar@acme.co"
      />
      <ContactCard
        name="Henderson G. Sterling II"
        job="Receptionist"
        email="henderson-the-second@acme.co"
      />
      <ContactCard
        name="Aoi Kobayashi"
        job="President"
        email="kobayashi.aoi@acme.co"
      />
    </ul>
  );
}

export default App;
```

#### Creating a "Button" component

- With the two `<button>` elements, Create a `Button` component and use it to render both buttons.

- Starter code:

```JAVASCRIPT
import { createRoot } from 'react-dom/client';

const root = createRoot(
  document.querySelector('#root'),
);

root.render(
  <div>
    <button
      style={{
        border: '2px solid',
        color: 'red',
        borderColor: 'red',
        background: 'white',
        borderRadius: 4,
        padding: 16,
        margin: 8,
      }}
    >
      Cancel
    </button>
    <button
      style={{
        border: '2px solid',
        color: 'green',
        borderColor: 'green',
        background: 'white',
        borderRadius: 4,
        padding: 16,
        margin: 8,
      }}
    >
      Confirm
    </button>
  </div>,
);
```

- Your solution:

- **Solution 1: Each dynamic attribute becomes a prop**

```JAVASCRIPT
import { createRoot } from 'react-dom/client';

function Button({ color, borderColor, children }) {
  return (
    <button
      style={{
        border: '2px solid',
        color,
        borderColor,
        background: 'white',
        borderRadius: 4,
        padding: 16,
        margin: 8,
      }}
    >
      {children}
    </button>
  );
}

const root = createRoot(
  document.querySelector('#root'),
);

root.render(
  <div>
    <Button
      color="red"
      borderColor="red"
    >
      Cancel
    </Button>
    <Button
      color="green"
      borderColor="green"
    >
      Confirm
    </Button>
  </div>,
);
```

- More advanced solutions
- Start to think about Component Design, not the UI, but how can we create this component, to be as friendly to developers and be able to reuse.
- What is the optimal API, for the props, to create the best experience.
- The first way, find the things that are different, and make them props, `color`, `borderColor`. And then just pass the props in.
- Other ways
- Do you really need two props for this?
- Since the colors are used multiple time, you could create a `themeColor` prop.

- **Solution 2: Synchronizing text and border colors**

```JAVASCRIPT
import { createRoot } from 'react-dom/client';

function Button({ themeColor, children }) {
  return (
    <button
      style={{
        border: '2px solid',
        color: themeColor,
        borderColor: themeColor,
        background: 'white',
        borderRadius: 4,
        padding: 16,
        margin: 8,
      }}
    >
      {children}
    </button>
  );
}

const root = createRoot(
  document.querySelector('#root'),
);

root.render(
  <div>
    <Button
      themeColor="red"
    >
      Cancel
    </Button>
    <Button
      themeColor="green"
    >
      Confirm
    </Button>
  </div>,
);
```

- The color signifies a meaning, green a confirmation, and red error.
- What is we had a status prop, cancel or confirm.
- And we use this status to determine the color.

- **Solution 3: Semantic "status" prop**

```JAVASCRIPT
// status: 'cancel' | 'confirm'
function Button({ status, children }) {
  let themeColor;

  if (status === 'cancel') {
    themeColor = 'red';
  } else if (status === 'confirm') {
    themeColor = 'green';
  } else {
    throw new Error('Unrecognized value');
  }

  return (
    <button
      style={{
        border: '2px solid',
        color: themeColor,
        borderColor: themeColor,
        background: 'white',
        borderRadius: 4,
        padding: 16,
        margin: 8,
      }}
    >
      {children}
    </button>
  );
}

root.render(
  <div>
    <Button
      status="cancel"
    >
      Cancel
    </Button>
    <Button
      status="confirm"
    >
      Confirm
    </Button>
  </div>,
);
```

- This way, we created some separation between the semantic meaning of this button and the particular color. So if you want to change things in the future, you can do it in one place.

### Application Structure

- Most React apps will share a common structure.

- An `index.js`, `App.js` and a `Header.js`
- See the [code sandbox](https://codesandbox.io/s/cehrev?file=/index.js&utm_medium=sandpack)

#### Index.js

- The root `index.js` file is the first bit of code to be executed. Responsible for rendering our React app, turning the elements we write into live DOM nodes.
- There will only be 1 spot int he entire codebase that calls the `createRoot` and `render` methods form react-dom.
- Common to do setup tasks in this file as well. Projects created with `create-react-app` include some performance, also you can include CSS files.
- Typically don't want to render a bunch of JSX, or include things like headers and buttons etc.
- General rule, just renders the single element: `<App />`

#### App

- Common for our projects to have a component called `App`
- This is the home base, React component in our project.
- Could manage core layout stuff, like headers and footers.
- If you are using a routing solution like react Router, top level routes are often included in this file.

#### Modules

- Generally use ES module system to split our apps into multiple files.
- For a refresher on JS modules

#### Going forward

- To keep things focused on the course, not showing `index.js`.

### Fragments

- Imagine you are writing the following HTML:

```HTML
<h1>Welcome to my homepage!</h1>
<p>Don't forget to sign the guestbook!</p>
```

- If we copy/paste that HTML into our React app, turning it into JSX, we get an error
- `Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>?`

- Why does this happen?
- 🤔 Hint: Try converting the JSX to `React.createElement` function calls.

- Why can't React simply return multiple elements?

```JAVASCRIPT
import React from 'react';

function App() {
    return (
        <h1>This is the heading</h1>
        <p>Welcome to this space!</p>
    );
}

export default App;
```

- In this format, it's not clear, but once you turn it into pure JS, it's more obvious.

```JAVASCRIPT
import React from 'react';

function App() {
  return (
    // works with just one
    React.createElement('h1', {}, 'Welcome')
    // fails with two
    React.createElement('p', {}, 'Sign in')
  );
}

export default App;
```

- In JS, the `return()` statement has one expression slot. You can return one expression, and it valid.
- **But you CANNOT add multiple expressions.**
- For example;

```JAVASCRIPT
let arr = [1, 2, 3];

return arr.push(4) arr.push(4) arr.push(4) arr.push(4)
```

- Looking at our React `App()`, imagine each element, `<h1>`, `<p>` as a function call.
- Then it makes more sense, you cannot just make two function calls side by side.

```JAVASCRIPT
function App() {
    return (
        <h1>This is the heading</h1>
        <p>Welcome to this space!</p>
    );
}
```

- **TIP: ALL OF THE JSX THAT WE WRITE, GETS TURNED INTO JS, AND HAS TO FOLLOW THE SAME RULES.**

- In react, there is a solution for this, it's called Fragments.

- So how do we solve for this?
- One option is to wrap both React elements in a div:

```JAVASCRIPT
return (
    <div>
      <h1>Welcome!</h1>
      <p>Don't forget to sign in</p>
    </div>
);
```

- This gets rendered to JS as, and we are no longer returning multiple expressions.

```JAVASCRIPT
return (
    React.createElement(
        'div',
        {},
        React.createElement('h1', {}, 'Welcome'),
        React.createElement('p', {}, 'Don\'t forget to sign guestbook'),
    )
)
```

- This is a fix, but not idea, and pollutes the DOM.
- It can also create accessibility and layout issues.

- **Better way with, Fragments**

- 🚀 A fragment is a special React component that doe snot product a DOM node. It looks like this:

```JAVASCRIPT
import React from 'react';

function App() {
  return (
    <React.Fragment>
      <h1>Welcome!</h1>
      <p>Don't forget to sign in.</p>
    </React.Fragment>
  );
}

export default App;
```

- If you inspect the output, you get the following;

```HTML
<body>
    <div id="root">
        <h1>Welcome!</h1>
        <p>Don't forget to sign in.</p>
    </div>
</body>
```

- Fragments allow us to respect the rules of JS without polluting the HTML.

#### Shorthand

- You can also create a react fragment with the following syntax:

```JAVASCRIPT
return (
  <>
    <h1>Welcome to my homepage!</h1>
    <p>Don't forget to sign the guestbook!</p>
  </>
);
```

### Iteration

- From a previous example; Building a CRM, we extracted a `ContactCard` component and used it for our 3 contacts:

```JAVASCRIPT
<ul>
  <ContactCard
    name="Sunita Kumar"
    job="Electrical Engineer"
    email="sunita.kumar@acme.co"
  />
  <ContactCard
    name="Henderson G. Sterling II"
    job="Receptionist"
    email="henderson-the-second@acme.co"
  />
  <ContactCard
    name="Aoi Kobayashi"
    job="President"
    email="kobayashi.aoi@acme.co"
  />
</ul>;
```

- This solution works, but we don't always have the data when we write the code.
- If we are building a CRM software, this data will be dynamic. Every user will have a separate set of contacts, and change over time. We can't hardcode.

- In React, we solve this by using iteration. We dynamically create these React elements by using raw JS.

#### Mapping Over Data

- Let's suppose the data from our CRM is held in an array.

```JAVASCRIPT
import ContactCard from './ContactCard';

const data = [
  {
    id: 'sunita-abc123',
    name: 'Sunita Kumar',
    job: 'Electrical Engineer',
    email: 'sunita.kumar@acme.co',
  },
  {
    id: 'henderson-def456',
    name: 'Henderson G. Sterling II',
    job: 'Receptionist',
    email: 'henderson-the-second@acme.co',
  },
  {
    id: 'aio-ghi789',
    name: 'Aoi Kobayashi',
    job: 'President',
    email: 'kobayashi.aoi@acme.co',
  },
];

function App() {
  return (
    <ul>
      <ContactCard
        name="Name goes here"
        job="Job goes here"
        email="Email goes here"
      />
    </ul>
  );
}

export default App;
```

- We want to create a `<ContactCard>` element for each of the contacts in the `data` array, passing in their name/job/email.

- In React, we use pure JS, there is no "React syntax" for doing this iteration.
- 🤔 HINT: You can render an array inside the JSX, React will unpack it for you. For a refresher, you can look at "[Array Iteration Methods](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/08-array-iteration-methods)" lesson.

- You can use the `map.()` method to iterate over the data

```JAVASCRIPT
const elements = data.map(contact => (
  console.log(contact.email)
));
```

- You can use, just vanilla JS, inside of React.
- Now, all you have to do is put in your component.
- Then use an expression slot `{elements}`, to render it in your `App()`

```JAVASCRIPT
import ContactCard from './ContactCard';

const data = [
  {
    id: 'sunita-abc123',
    name: 'Sunita Kumar',
    job: 'Electrical Engineer',
    email: 'sunita.kumar@acme.co',
  },
  {
    id: 'henderson-def456',
    name: 'Henderson G. Sterling II',
    job: 'Receptionist',
    email: 'henderson-the-second@acme.co',
  },
  {
    id: 'aio-ghi789',
    name: 'Aoi Kobayashi',
    job: 'President',
    email: 'kobayashi.aoi@acme.co',
  },
];

function App() {
  const elements = data.map(contact => (
    <ContactCard
      name={contact.name}
      job={contact.job}
      email={contact.email}
    />
  ));
  return (
    <ul>
      {elements}
    </ul>
  );
}

export default App;
```

- If you console.log(elements), you will see that you are creating an array of React elements, and inserting them.

- It is more common to put the JSX inline, so everything happens inline and easier to determine what is happening.
- We are going to `map()` over the data, and render a contact card for every contact we have.

```JAVASCRIPT
function App() {
  return (
    <ul>
      {data.map(contact => (
        <ContactCard
          name={contact.name}
          job={contact.job}
          email={contact.email}
        />
      ))}
    </ul>
  );
}
```

- You can do anything you can do in JS. Can use the JS you already know.

- 🎁 Bonus material: JS primer bonus
- [Arrow Functions](https://courses.joshwcomeau.com/joy-of-react/01-fundamentals/06.01-mapping?peek=%2Fjoy-of-react%2F10-javascript-primer%2F04-arrow-functions)
- [Array Iteration Methods](https://courses.joshwcomeau.com/joy-of-react/01-fundamentals/06.01-mapping?peek=%2Fjoy-of-react%2F10-javascript-primer%2F08-array-iteration-methods)

#### JSX inside JS inside JSX

- When iterating with React, it is not uncommon to wind up with a structure like this:

```JAVASCRIPT
<ul>
  {items.map(item => (
    <li>{item}</li>
  ))}
</ul>;
```

- On the second line, we use curly brackets to add some "vanilla JS", to our JSX. But then we are using JSX inside those curly brackets!
- The JSX compiler is able to resolve the "nested" JSX calls. The end result is something like this:

```JAVASCRIPT
React.createElement(
  'ul',
  {},
  items.map(item => (
    React.createElement(
      'li',
      {},
      item,
    )
  )),
);
```

#### Keys

- The good ole keys console warning.
- `Warning: Each child in a list should have a unique "key" prop.`

- When we give React an array of elements, we also need to help react otu b uniquely identifying each element.
- In our CRM application, we can fix this by using the `id` as the key prop.

```JAVASCRIPT
const data = [
  {
    id: 'sunita-abc123',
    name: 'Sunita Kumar',
    job: 'Electrical Engineer',
    email: 'sunita.kumar@acme.co',
  },
  // ✂️ Other contacts trimmed
];
function App() {
  return (
    <ul>
      {data.map(contact => (
        <ContactCard
          key={contact.id}
          name={contact.name}
          job={contact.job}
          email={contact.email}
        />
      ))}
    </ul>
  );
}
```

- Our contacts data array has a unique identifier, `id`.
- We pass that string onto the `key` property, to resolve the error.

#### Why are keys necessary?

- Why can't React figure this out on it's own?
- You need keys, to help react understand exactly how the data shifts over time.
- When updating the DOM, there are multiple ways to get from one UI to another. And if you give React unique identifiers, we can see how those things change 🤔
- [Video explanation](https://courses.joshwcomeau.com/joy-of-react/01-fundamentals/06.02-keys)

#### Using the array index as the key?

- What if you are not given a unique id for each contact, like the example above.
- A common approach is to use the array index. The index is provided when we call the `.map()` method.

```JAVASCRIPT
const todos = [
  'buy milk',
  'feed goldfish',
  'return library book',
];

function TodoList() {
  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index}>{todo}</li>
      ))}
    </ul>
  );
}
```

- As we iterate through teh array, we are grabbing each items index, and applying it as a key. The output in React is like:

```JAVASCRIPT
<li key={0}>buy milk</li>
<li key={1}>feed goldfish</li>
<li key={2}>return library book</li>
```

- This can be a bit risky, React keys are meant to be uniquely identity a bit of content.
- In the re-order case, you could end up with something like this.

```JAVASCRIPT
<li key={0}>feed goldfish</li>
<li key={1}>buy milk</li>
<li key={2}>return library book</li>
```

- This could cause some bugs in your app.
- A good rule, is to not use `key` with array index if the items can be reordered.

#### Key Rules

- Some rules that govern how keys should be used.

##### Top level element

- the `key` prop needs to be applied ot hte very top-level element within the `.map()` call.
- This example is incorrect.

```JAVASCRIPT
function NavigationLinks({ links }) {
  return (
    <ul>
      {links.map(item => (
        <li>
          <a
            key={item.id} // incorrect element
            href={item.href}
          >
            {item.text}
          </a>
        </li>
      ))}
    </ul>
  );
}
```

- From React's perspective, it has a group of `<li>` elements, does not dig any deeper at the child elements.
- To fix it, just move the `key` up to the `<li>`.

- When using fragments, it's sometimes required to switch to the long form `react.Fragment`, so you can apply the key.

```JAVASCRIPT
// 🚫 Missing key: 
function Thing({ data }) {
  return (
    data.map(item => (
      <>
        <p>{item.content}</p>
        <button>Cancel</button>
      </>
    ))
  );
}

// ✅ Fixed!
function Thing({ data }) {
  return (
    data.map(item => (
      <React.Fragment key={item.id}>
        <p>{item.content}</p>
        <button>Cancel</button>
      </React.Fragment>
    ))
  );
}
```

##### Not global

- Keys only have to be unique within their array, the `key` prop does not have to be globally unique  across the entire application.
- Each `.map()` cal produces a separate array, and so it's not a problem.

#### Exercises, Iteration

- Some exercises to get practice on what we went over.

#### Avatar selection

- Update the multiple avatars.
- AC's
  - Create an array that holds the data needed for all the avatars.
  - The array should be iterated over, creating an `<Avatar />` element for each item in the array.
  - Should be no key warnings in the console.

```JAVASCRIPT
// start
import Avatar from './Avatar';

function App() {
  return (
    <div className="avatar-set">
      <Avatar
        src="https://sandpack-bundler.vercel.app/img/avatars/001.png"
        alt="person with curly hair and a black T-shirt"
      />
      <Avatar
        src="https://sandpack-bundler.vercel.app/img/avatars/002.png"
        alt="person wearing a hijab and glasses"
      />
      <Avatar
        src="https://sandpack-bundler.vercel.app/img/avatars/003.png"
        alt="person with short hair wearing a blue hoodie"
      />
      <Avatar
        src="https://sandpack-bundler.vercel.app/img/avatars/004.png"
        alt="person with a pink mohawk and a raised eyebrow"
      />
    </div>
  );
}

export default App;
```

- AC 1. Create an array that holds the data needed for all the avatars.
- Note: since the src url is the same, good practice to just specify the name of the image as an `id`.

```JAVASCRIPT
const data = [
  {
    id: '001',
    alt: 'person with curly hair and a black T-shirt',
  },
  {
    id: '002',
    alt: 'person wearing a hijab and glasses',
  },
  {
    id: '003',
    alt: 'person with short hair wearing a blue hoodie',
  },
  {
    id: '004',
    alt: 'person with a pink mohawk and a raised eyebrow',
  },
];
```

- AC 2. The array should be iterated over, creating an `<Avatar />` element for each item in the array.
- Note: Use string interpolation with template strings for the image url.
- Use an arrow function on the map method. `(data) => (return stuff)`, don't forget about the rules of an arrow function.
- In the function return, you can just call the expression, with slot.

```JAVASCRIPT
function App() {
  // iterate over
  const element = data.map(avatar => (
    // console.log(data);
    <Avatar
      src={`https://sandpack-bundler.vercel.app/img/avatars/${avatar.id}.png`}
      alt={avatar.alt}
    />
  ));

  return (
    <div className="avatar-set">
      {element}
    </div>
  );
}
```

- Note, Josh's solution, you can put the code inline within the return. Not make a separate variable.

```JAVASCRIPT
function App() {
  return (
    <div className="avatar-set">
      {data.map(avatar => (
        // console.log(data);
        <Avatar
          src={`https://sandpack-bundler.vercel.app/img/avatars/${avatar.id}.png`}
          alt={avatar.alt}
        />
      ))}
    </div>
  );
}
```

- AC 3. No Key warnings.

```JAVASCRIPT
<Avatar
  src={`https://sandpack-bundler.vercel.app/img/avatars/${avatar.id}.png`}
  alt={avatar.alt}
  key={avatar.id}
/>;
```

- Getting fancy
- Use destructuring to simplify the return
- Arrow function, implicit return

```JAVASCRIPT
function App() {
  return (
    <div className="avatar-set">
      {data.map(({ id, alt }) => (
        <Avatar
          src={`https://sandpack-bundler.vercel.app/img/avatars/${id}.png`}
          alt={alt}
        />
      ))}
    </div>
  );
}
```

#### Shopping Cart

- Imagine building a shopping cart UI. You receive an array of items being held in teh card from the server.
- If an item is otu of stock, they cannot purchase it. Should be displayed separately.

- Started on, but two problems remain:
  - Need to show all the items in teh user's cart, not just the first one.
  - Need to show two separate tables. One for in-stock, and one for sold-out.

- AC's
  - Update the `CartTable` component to use iteration.
  - Make sure no `key` warnings in the console.
  - In `App`, we should be rendering two `CardTable` elements:
    - One for the in-stock elements
    - One for the out-stock elements below the "Sold Out" heading.

- AC 1. Update the `CartTable` component to use iteration.
- AC 2. Make sure no `key` warnings in console

```JAVASCRIPT
// CartTable.js
function CartTable({ items }) {
  // TODO: Map through “items”, creating 1 row
  // per item.

  const productRow = items.map(({
    id, imageSrc, imageAlt, title, price,
  }) => (
    // console.log(imageSrc)
    <tr className="cart-row" key={id}>
      <td>
        <img
          className="product-thumb"
          src={imageSrc}
          alt={imageAlt}
        />
      </td>
      <td>
        {title}
      </td>
      <td>
            $
        {price}
      </td>
    </tr>
  ));

  return (
    <table className="shopping-cart">
      <thead>
        <tr>
          <th />
          <th>Title</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>
        {productRow}
      </tbody>
    </table>
  );
}

export default CartTable;
```

- AC 3. In `App`, we should be rendering two `CardTable` elements:
  - One for the in-stock elements
  - One for the out-stock elements below the "Sold Out" heading.

- 🤔 TIP: Use the `filter()` method on the list of items. Pass that into your `<CardTable>` component.

- Pass that into your `<CardTable>` component.
- 🤔 TIP: You can invert the condition so it will return falsy `!item.inStock`.

```JAVASCRIPT

const items = [
  {
    id: 'hk123',
    imageSrc: 'https://sandpack-bundler.vercel.app/img/shopping-cart-coffee-machine.jpg',
    imageAlt: 'A pink drip coffee machine with the “Hello Kitty” logo',
    title: '“Hello Kitty” Coffee Machine',
    price: '89.99',
    inStock: true,
  },
  // other items here
];

function App() {
  const inStockItems = items.filter(
    item => item.inStock,
  );
  const outOfStockItems = items.filter(
    item => !item.inStock,
  );

  return (
    <>
      <h2>Shopping cart</h2>
      <CartTable items={inStockItems} />
      <div className="actions">
        <button>Continue checkout</button>
      </div>

      <h2>Sold out</h2>
      <CartTable items={outOfStockItems} />
    </>
  );
}

export default App;
```

### Conditional Rendering

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

### Styling in React

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

## Module 2 - Working with State

- In the early days of the web, websites were basically fancy documents, we would load one HTML file, read the content and then load another one.

- The modern web is interactive. The app can respond to user actions in real time. Without the need to fetch the whole page.

- In this module:
  - How to respond to user actions with event binding.
  - How React manages the DOM for us, and what it means to re-render.
  - The `useState` hook and how to use it to build interactive components.
  - Understand the difference between props and state.
  - Working with forms in React.
  - Working with complex state, like objects and arrays.
  - Avoiding common pitfalls, state mutation.
  - How to share state across the application by lifting it up.

- Going from static websites to dynamic alive apps!

### Event Handlers

- When a user interacts with the page, hundreds of events are fired off in response. The browser is tracking every little thing you do.

- These events are important when building dynamic applications, we will listen for these events and use them to trigger state changes.
- When the use clicks on "X" button, we dismiss teh prompt, when user submits the form, we show a loading spinner.

- In order to respond to an event, we need to listen for it. JS already provides a built in way to do this, `addEventListener` method:

```JAVASCRIPT
const button = document.querySelector('.btn');

function doSomething() {
  // stuff here
}

button.addEventListener('click', doSomething);
```

- In the code above, we are listening for a specific event `click` targeting a specific element `btn`. We have a function to handle this event `doSomething`.
- When the user clicks on this particular button, your handler function will be invoked, allowing us to do something in response.

- The web platform offers another way to do this. We can embed our handler right in the HTML

```HTML
<button onclick="doSomething()">
  Click me!
</button>
```

- Rect piggybacks on this pattern, allowing us to pass an event handler right into the JSX:

```JAVASCRIPT
function App() {
  function doSomething() {
    // Stuff here
  }

  return (
    <button onClick={doSomething}>
      Click me!
    </button>
  )
}
```

- As with the `addEventListener`, this code will do the same thing: when use clicks on the button, the `doSomething` function is called.

- **This is the recommended way to handle events in React.**
- Try and use the "onX" props like `onClick` and `onChange` when possible.

- Few reasons why:
  - **Automatic cleanup:** When we use an event listener, we supposed to remove it when we are done, with `removeEventListener`. If we forget to do this, we introduce a memory leak. React automatically removes listeners when we use 'on X' handler functions.
  - **Improved performance:** React can optimize things for use, like batching multiple event listeners together to reduce the memory consumption.
  - **No DOM interaction:** Avoid interacting with the DOM directly.

- ℹ️ One of the core ideas behind React, is that is does the DOM manipulation for you. Stay within React's abstraction rather than trying to compete with it to manage the DOM.

#### Differences from HTML, Events

- Looking at the `onChange` prop, it looks very similar tot he in HTML method of adding event handlers.

##### Camel Casing

- In JSX, you have to write `camelCased` attribute names in JSX.
- Be careful to write `onClick` instead of `onclick`. Other examples, `onChange`, `onKeyDown`, `onTransitionEnd` etc.
- If you forget, React will let you know.

##### Passing a function reference

- When working with event handlers in React, we need to pass a reference to the function. We don't call the function like we do in HTML:

```HTML
// ✅ We want to do this:
<button onClick={doSomething} />

// 🚫 Not this:
<button onClick={doSomething()} />
```

- When we include the parentheses, we invoke th function right away, the moment the React app is rendered. We want to give React a reference to the function, so React can call it at a later time, when the user clicks on the button.

#### Specifying Arguments

- Here is the thing, what if we want to specify an argument to a function?

- For example, if you wanted to make a function called `setTheme`, and we use it to change the user's color theme from Light Mode, to Dark Mode.

- To do this, we need to supply he name of the theme we are switching to, like this:

```JAVASCRIPT
// Switch to light mode:
setTheme('light');

// Switch to dark mode:
setTheme('dark');
```

- How can we do this? If we pass `setTheme` as a reference to the `onClick`, we can't supply arguments:

```JAVASCRIPT
// 🚫 Invalid: calls the function without 'dark'
<button onClick={setTheme}>
  Toggle theme
</button>
```

- In order to specify the argument, we need to wrap it in a parentheses, but it gets called right away:

```JAVASCRIPT
// 🚫 Invalid: calls the function right away
<button onClick={setTheme('dark')}>
  Toggle theme
</button>
```

- We can solve this problem with a wrapper function

```JAVASCRIPT
<button onClick={() => setTheme('dark')}>
  Toggle Theme
</button>
```

- We are creating a new anonymous arrow function, `() => setTheme('dark')` and passing it to React, to be called when the user clicks the button.

- When the button is clicked, the function is called and the code inside the function is executed: `setTheme('dark')`.

- For a refresher, check out the [Arrow Function Primer](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/04-arrow-functions) lesson 👀

#### Exercises, Event Handlers

##### Click the ball, build a simple game

- The goal of the game is to click the ball, to show an alert when the ball is clicked:

- We can us the `window.alert()` to show the message.
- AC's
  - When the user click on the ball, a winning message should be shown.
  - You should handle 'click' events specifically, as this even is triggered on click, on tap, or even when a user focuses on teh element with the keyboard and hits "Enter" Key
    - If you don't use a pointer device, you can use the keyboard method to test your code.

```JAVASCRIPT
import React from 'react';

import VisuallyHidden from './VisuallyHidden';

function ClickBallGame() {
  return (
    <div className="wrapper">
      <button
        className="ball"
      >
        <VisuallyHidden>
          Ball
        </VisuallyHidden>
      </button>
    </div>
  );
}

export default ClickBallGame;
```

- Solution:
- ℹ️ You can write as a separate function, especially is you have multiple expressions inside your function.

```JAVASCRIPT
import React from 'react';

import VisuallyHidden from './VisuallyHidden';

function ClickBallGame() {
  function handleClick() {
    window.alert('You win');
  }
  return (
    <div className="wrapper">
      <button
        className="ball"
        onClick={handleClick}
      >
        <VisuallyHidden>
          Ball
        </VisuallyHidden>
      </button>
    </div>
  );
}

export default ClickBallGame;
```

- You can also do this inline function, to achieve the same result.

```JAVASCRIPT
  return (
    <div className="wrapper">
      <button
        className="ball"
        onClick={() => window.alert('You win')}
      >
        <VisuallyHidden>
          Ball
        </VisuallyHidden>
      </button>
    </div>
  );
```

##### Click the ball v2

- Now, in addition to the ball, it now has a bomb. If the bomb is clicked, we want to show a 'lose' message.
- The problem with the started code, clicking on either the item, the ball or bomb, it shows the 'lose' message.

- You mission, if you choose it, it to fix the code so that i throws the right message depending on which item is clicked.

- AC's
  - When the user clicks the ball, a winning message should be shown.
  - When the user clicks the bomb, a losing message should be shown.
  - The `handeClick` function should still be used, and you shouldn't have to change anything about the function itself.

```JAVASCRIPT
// STARTER CODE
import React from 'react';

import VisuallyHidden from './VisuallyHidden';

function ClickBallGame() {
  function handleClick(type) {
    if (type === 'win') {
      alert('You win!');
    } else {
      alert('You lose :(');
    }
  }
  
  return (
    <div className="wrapper">
      <button
        className="ball"
        onClick={handleClick}
      >
        <VisuallyHidden>
          Ball
        </VisuallyHidden>
      </button>
      <button
        className="bomb"
        onClick={handleClick}
      >
        <span
          role="img"
          aria-label="bomb"
        >
          💣
        </span>
      </button>
    </div>
  );
}

export default ClickBallGame;
```

- My solution:
- Put a wrapper function on the `onClick` and pass in the `type` parameter into the `handleClick()` function.

```JAVASCRIPT
  return (
    <div className="wrapper">
      <button
        className="ball"
        onClick={() => handleClick('win')}
      >
        <VisuallyHidden>
          Ball
        </VisuallyHidden>
      </button>
      <button
        className="bomb"
        onClick={() => handleClick('lose')}
      >
        <span
          role="img"
          aria-label="bomb"
        >
          💣
        </span>
      </button>
    </div>
  );
```

### The useState Hook

- Below is a common example of a minimal counter demo.
- Click on the button and watch the count increase:

```JAVASCRIPT
import React from 'react';

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Value: {count}
    </button>
  );
}

export default Counter;
```

- Break this down, what is going on here:
- Our goal is to keep track of the number of times the user has clicked the button.
- For dynamic values, we need to use React state.
- State is used for values that change over time.

- To create a state variable, we use the `useState` function.
- This function takes a single argument: the initial value. In this case, that value is `0`. The value is chosen because when the page first loads, we clicked the button 0 times.

- `useState` is a **hook**. A hook is a special type of function that allows us to 'hook into' React internals.

- The `useState` hook returns an array containing two items:

1. The current value of the state variable. We've decided to call it `count`, this can be whatever you want to name it.
2. A function we can use to update the state variable. We named it `setCount`. It's best practice to pre-pend the `set` to the function.

- ℹ️ Destructing Assignment, below is the same thing as the above `const [count, setCount]` we are destructuring `count` and `setCount`.

```JAVASCRIPT
const countArray = React.useState(0);

const count = countArray[0];
const setCount = countArray[1];
```

- 👀 Can check out the [Array Destructuring Primer Lesson](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/06-array-destructuring)

##### Naming Conventions

- When we create a state variable, we can name it whatever we want.
- However, it's best practice to follow the `x`, `setX` convention:

```JAVASCRIPT
const [user, setUser] = React.useState();
const [errorMessage, setErrorMessage] = React.useState();
const [flowerBouquet, setFlowerBouquet] = React.useState();
```

- `user` The first destructured variable si the name of the thing we are tracking.
- `setUser` The second variable prefixes that name with the `set`, specifying it's a function that can be called to change the thing. This is sometimes referred to as a "setter function", since it sets the new value of teh state variable.

- ℹ️ Importing the hook?
  - Some tutorials write this code differently.
  - Import at the top of the page. `import React, { useState } from 'react';`
  - Instead of `React.useState`, you can import `useState` function at the top of the file and using it on it's own, `useState`.

##### Initial Value

- React state variables can be given an initial value:

```JAVASCRIPT
const [count, setCount] = React.useState(1);
console.log(count);
```

- We can also supply a function. React will call this function on the very first render to evaluate the initial value:

```JAVASCRIPT
const [count, setCount] = ReactuseState(() => {
  return 1 + 1;
});

console.log(count); // 2
```

- Calling a function can be useful to do an expensive operation to calculate the initial value. For example, reading from localStorage:

```JAVASCRIPT
const [count, setCount] = React.useState(() => {
  return window.localStorage.getItem('saved-count');
});
```

- The benefit here is that we are only doing the expensive work (redon from localStorage) once, on the initial render, rather than doing it on every single render.

- In a later lesson, discuss localStorage

- ℹ️ Hold up, What's the difference between those two?

```JAVASCRIPT
// Form 1:
const [count, setCount] = React.useState(
  window.localStorage.getItem('saved-count')
);

// Form 2:
const [count, setCount] = React.useState(() => {
  return window.localStorage.getItem('saved-count');
});
```

- Helpful to forget about React for a moment and think in pure JS.

- Consider this function:

```JAVASCRIPT
function run() {
  console.log('Hello');
}
```

- When we call the `run` function, we will log the word "Hello", if I call the function 5 times, I get 5 "Hello" in the console.

- Tweak it:

```JAVASCRIPT
function run() {
  const sayHi = () => {
    console.log('Hello');
  };
}
```

- Now, when I call the `run` function, nothing is logged to the console. That's because the `console.log` is wrapped up in an inner function, `sayHi`.
- every time we call the `run` function, we are creating a brand new `sayHi` function, but never calling it.

#### Core React Loop

- We have seen, when we update the state variable by calling the setter function `setCount` the UI gets updated. But how does this actually work?

- This is the core of React, literally named for how it reacts to state changes.

- Let's use the counter as an example:

```JAVASCRIPT
function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Value: {count}
    </button>
  )
}
```

- What is happening here exactly when the component is rendered for the first time?
- Our `Counter` function returns a bunch of JSX, Let's look at it in pure JS, so see what's going on.

```JAVASCRIPT
function Counter() {
  const [count, setCount] = React.useState(0);

  return React.createElement(
    'button',
    { onClick: () => setCount(count + 1) },
    'Value: ',
    count
  );
}
```

- When this code runs, `React.createElement` produces a React element, which is a plain JS object like this:

```JAVASCRIPT
{
  type: 'button',
  key: null,
  ref: null,
  props: {
    onClick: () => setCount(count + 1),
  },
  children: 'Value: 0',
  _owner: null,
  _store: { validated: false }
}
```

- As we learned before, React elements are essentially descriptions of the UI we want. Here we are saying we want a button that contains the text "Value: 0".
- We could visualize this JS object as the following HTML snippet:

```HTML
<button>
  Value: 0
</button>
```

- Our React element, that JS object, is describing teh DOM structure. React takes that description and turns it into a real thing.
- It creates a `<button>` DOM node and put on the page.

- I didn't show the `onClick` handler in this little sketch, but it's very much a part of this process. When react creates and injects the `<button>` DOM node, it attaches our handler function.

- **Let's think about what happens when the button is clicked.**
- The `setCount` function will be called and we will pass in a new value. `count` will be incremented from 0 to 1.

- **Whenever a state variable is updated, it triggers a re-render.**
- Once again, React will cal the `Counter` function.
- This creates a brand-new React element, a new description of the UI we want.

- The new react element describes this DOM structure:

```HTML
<button>
  Value: 1
</button>
```

- **Each render is like taking a snapshot.**
- We generate a description that shows what the UI should look like, based on the components props/state. Like a photo that captures what things were like at a moment in time.
- And React has two snapshots:

```HTML
<button>
  Value: 0
</button>
```

```HTML
<button>
  Value: 1
</button>
```

- The user clicked a button and this second snapshot was generated.
- React now has to figure out how to update the DOM, so that is matches this latest snapshot.
- It's like those puzzle games, where you have to figure out the differences between two images.
- React has to play this sort of game, looking for the changes between teh two snapshots.

- **This process is known as reconciliation.**
- React figures out what's changed and updates the UI.
- Once React figures out the difference, it will need to **commit** these changes. With precision, it updates teh DOM, taking care only to tweak the things hat need to be tweaked.

In our button case, the operation would be something like:

```JAVASCRIPT
button.innerText = "Value: 1";
```

- **This is the fundamental "flow" of React, the core loop.**
- The sequence is like:

- Mount
  - Trigger > Render > Commit

- Mount: When we render the component for the first time, there is no previous snapshot to compare it to. So, React will create al the necessary DOM nodes from scratch, and inject them into the page.

##### Rendering vs. Painting

- Like 3D software, when you take raw data and turn it into an image, like a 3D model in Blender.
- The term "render" generally refers to this sort of thing; we are taking some sort of unprocessed raw input, and generating a final ready to use output.

- Even in web frameworks, the definition is pretty consistent.

- In React, however the term "render" means something slightly different. I think so much of the confusion in React comes from this misunderstanding.

- In this example:

```JAVASCRIPT
function AgeLimit({ age }) {
  if (age < 18) {
    return (
      <p>You're not old enough!</p>
    );
  }

  return (
    <p>Hello, adult!</p>
  );
}
```

- Our `AgeLimit` component check an `age` prop and returns one of two paragraphs.
- Now, let's suppose we re-render this component, and wind up with the following before/after pair of snapshots:

```JAVASCRIPT
age: 16

{
  type: 'p',
  key: null,
  ref: null,
  props: {},
  children: "You're not old enough!",
}
```

```JAVASCRIPT
age: 17

{
  type: 'p',
  key: null,
  ref: null,
  props: {},
  children: "You're not old enough!",
}
```

- In both cases, `age` is less than 18, and so we wind up with teh exact same UI.
- As a result, *no DOM mutation* happens at all.

- When we talk about 're-rendering', we are saying to check it anything's changed.
- 💡 If we spot a difference between snapshots, React will need to update the DOM, but it will be a precisely-targeted minimal change. 💡

- When React does change a part of the DOM, teh browser will need to re-paint. A re-paint is when the pixels on the screen are re-drawn because a part of the DOM was mutated.

- This is done natively by the browser when teh DOM is edited with JS, whether by React, Angular, jQuery, vanilla JS, etc.

To Summarize: 🚀 React Re-Rendering

- A **re-render** is a React process where we figure out what needs to change, AKA reconciliation, to spot the difference between snapshots.

- If something has changed between eh two snapshots, React will "commit" those changes by deleting the DOM, so that it matches the latest snapshot.

- Whenever a SOM node is edited, the browser will re-paint, re-drawing the relevant pixel so that the user sees the correct UI.

- **No all re-renders require re-paints!** If nothing has changed between snapshots, React won't edit any DOM nodes, and nothing will be re-painted.

- 👀 The critical thing to understand si that when we talk about re-rendering, we are not saying that we should throw away the current UI and re-build everything from scratch.

- React ties to keep the re-painting to a minimum, because re-painting is slow. Instead of generating a bunch of new DOM notes from scratch (lots of painting), it figures out what's changed between snapshots and make the required tweaks with surgical precision.

- For more in-depth; see react docs [Render and Commit](https://react.dev/learn/render-and-commit)

#### Asynchronous Updates

- Consider the following code: What value would you expect to see in the developer console when the user clicks the button for the first time?

```JAVASCRIPT
function App() {
  const [count, setCount] = React.useState(0);

  return (
    <>
      <p>
        You've clicked {count} times.
      </p>
      <button
        onClick={() => {
          setCount(count + 1);
          console.log(count)
        }}
      >
        Click me!
      </button>
    </>
  );
}
```

- The answer is 0, I guessed 1, what is going on here?

- When we create our state variable, we initialize ti to `0`. Then, when we click the button, we increment it by 1, to `1`. So shouldn't it log `1` and not `0`?

- Here is the catch: **state setters are not immediate.**

- When we call `setCount` we tell Rect that we would like to request a change to a state variable.
- React does not immediately drop everything; it waits until the current operation is completed (processing the click), and then updates the value and triggers a re-render.

- For now, the thing to remember is that **updating a state variable is asynchronous**. It affects what the state will be for the **next render**. It's a scheduled update.

- Here is how to fix the code, so that we have access to the new value right away:

```JAVASCRIPT
function App() {
  const [count, setCount] = React.useState(0);

  return (
    <>
      <p>
       You clicked {count} times.
      </p>
      <button
        onClick={() => {
          const nextCount = count + 1;
          setCount(nextCount);
          console.log(nextCount)
        }}
      >
        Click me!
      </button>
    </>
  )
}
```

- We store the updated value in a variable, so that we can access that variable whenever we want to know what the new value is.

- I like to use prefix `next`, since it conveys that we are talking about the 'future' value of the state, what it will be on the next render. But you can name whatever you want.
- Try and fix the bug:

#### Exercises, useState Hook, Role Playing Game

- You are building an in-browser role playing game, but we have a bug. The wrong values are being shown to the user when the character levels up.

```JAVASCRIPT
import React from 'react';

function Character() {
  const [strength, setStrength] = React.useState(6);
  const [dexterity, setDexterity] = React.useState(9);
  const [intelligence, setIntelligence] = React.useState(15);

  function triggerLevelUp() {
    setStrength(strength + 1);
    setDexterity(dexterity + 2);
    setIntelligence(intelligence + 3);

    window.alert(`
      Congratulations! Your hero now has the following stats:
      Str: ${strength}
      Dex: ${dexterity}
      Int: ${intelligence}
    `);
  }

  return (
    <div className="wrapper">
      <img
        alt="8-bit wizard character"
        src="https://sandpack-bundler.vercel.app/img/mage-sprite.gif"
      />
      <button onClick={triggerLevelUp}>
        Level Up
      </button>
    </div>
  );
}

export default Character;
```

- My solution: To declare variables for the values so they update right away:
- What is going on with this code?
- Why are we seeing stale values when you click on the 'Level Up' button?
- It's not actually changing the variable as you would expect, `strength = strength + 1`
- The way the `triggerLevelUp()` function works in state, is they schedule an update.
- It's a way to tell React, hey we are changing some stuff, increase this value by 1.
- So please, re-render with this new value.
- Telling React to redo all the work that it has already done, to determine what the UI is.
- And on the next pass, next render, it should use the new values.
- When React sees `setStrength(strength + 1);`, React makes a "calendar appointment" but *it doesn't interrupt what it is already doing*, running through the function.
- **It's going to go through the rest of the function, without changing anything.**

- And the next time we run the function `Character`, the new values will be populated.

```JAVASCRIPT
function Character() {
  const [strength, setStrength] = React.useState(6);
  const [dexterity, setDexterity] = React.useState(9);
  const [intelligence, setIntelligence] = React.useState(15);

  function triggerLevelUp() {
    const nextStrength = strength + 1;
    const nextDexterity = dexterity + 2;
    const nextIntelligence = intelligence + 3;
    
    setStrength(nextStrength);
    setDexterity(nextDexterity);
    setIntelligence(nextIntelligence);

    window.alert(`
      Congratulations! Your hero now has the following stats:
      Str: ${nextStrength}
      Dex: ${nextDexterity}
      Int: ${nextIntelligence}
    `);
  }
```

#### Exercise, Counter 2.0

- The buttons have already been added to the page, but they don't do anything yet. The task is to wire up the buttons with the following ACs.

ACs:

- The up arrow button should increase the count by 1.
- The double arrow up button should increase the count by 10.
- The refresh button should reset the count to 0.
- The pound button should set the count to a random number between 1 and 100.
- The double down arrow button should decrease the count by 10.
- The down arrow button should decrease the count by 1.

- Starter code:

```JAVASCRIPT
import { ChevronUp, ChevronsUp, ChevronDown, ChevronsDown, RotateCcw, Hash } from 'react-feather'

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <div className="wrapper">
      <p>
        <span>Current value:</span>
        <span className="count">
          {count}
        </span>
      </p>
      <div className="button-row">
        <button>
          <ChevronUp />
          <span className="visually-hidden">
            Increase slightly
          </span>
        </button>
        <button>
          <ChevronsUp />
          <span className="visually-hidden">
            Increase a lot
          </span>
        </button>
        <button>
          <RotateCcw />
          <span className="visually-hidden">
            Reset
          </span>
        </button>
        <button>
          <Hash />
          <span className="visually-hidden">
            Set to random value
          </span>
        </button>
        <button>
          <ChevronsDown />
          <span className="visually-hidden">
            Decrease a lot
          </span>
        </button>
        <button>
          <ChevronDown />
          <span className="visually-hidden">
            Decrease slightly
          </span>
        </button>
      </div>
    </div>
  );
}

export default Counter;
```

- The solution: use the `setCount` **state setter function** to increase and decrease.

```JAVASCRIPT
import React from 'react';
import { ChevronUp, ChevronsUp, ChevronDown, ChevronsDown, RotateCcw, Hash } from 'react-feather'

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <div className="wrapper">
      <p>
        <span>Current value:</span>
        <span className="count">
          {count}
        </span>
      </p>
      <div className="button-row">
        <button
          // Increase by 1
          onClick={
            () => setCount(count + 1)
          }
        >
          <ChevronUp />
          <span className="visually-hidden">
            Increase slightly
          </span>
        </button>
        <button 
          // Increase by 10
          onClick={
            () => setCount(count + 10)
          }
        >
          <ChevronsUp />
          <span className="visually-hidden">
            Increase a lot
          </span>
        </button>
        <button
          onClick={
            () => setCount(0)
          }
        >
          <RotateCcw />
          <span className="visually-hidden">
            Reset
          </span>
        </button>
        <button
          // set random value
          onClick={
            () => setCount(Math.ceil(Math.random() * 100))
          }
        >
          <Hash />
          <span className="visually-hidden">
            Set to random value
          </span>
        </button>
        <button
          //decrease by 10
          onClick={
            () => setCount(count - 10)
          }
        >
          <ChevronsDown />
          <span className="visually-hidden">
            Decrease a lot
          </span>
        </button>
        <button
          // decrease by 1
          onClick={
            () => setCount(count - 1)
          }
        >
          <ChevronDown />
          <span className="visually-hidden">
            Decrease slightly
          </span>
        </button>
      </div>
    </div>
  );
}

export default Counter;
```

#### Why the dance?

- Why so much to manage state management in React?
- React is based on the confines of JavaScript.
- JS doesn't give us a way to observe variables.
- We need to know, when a variable changes in order for React to trigger a re-render.
- I our counter example, the only way for the UI to update, is for all the code to re-run. It doesn't automatically re-run the code when you change a variable.

```JAVASCRIPT
import React from 'react';

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Value: {count}
    </button>
  );
}

export default Counter;
```

- How do you get the increment, if you try in plain JS. You can't, it doesn't persist across function calls.

```JAVASCRIPT
function helloWorld() {
  let count = 0;
  count = count + 1;
}

helloWorld(); // 1
helloWorld(); // 1
helloWorld(); // 1
helloWorld(); // 1
```

- every time you call that function, you get the same results, it doesn't increment. We re-initialize the variable and increment by one.
- The increment `count + 1` doesn't persist across function calls. JS doesn't give us a way to thread values through function calls.

- Back to React, **Components in React are Functions in JS.**
- If you think through what the `Counter()` function above does.

```JAVASCRIPT
// Start you app
ReactDOM.render(<App />)
// Transpile to
ReactDOM.render(React.createElement(App))
// Calling this function, get an JS object with all the stuff
{ type: App, props {}}
// The point is, React is going to evoke the App() function
// Re-render
App()
// Re-render
App()
// Re-render
App()
// Re-render
App()

```

- The point is, React is going to evoke the `App()` function, every time we trigger a re-render.
- every time you re-render, it's like a timeline, a snapshot of that component.
- The variable needs to be defined within the component, so every instance can have it's own dynamic state.

- React exist in the confines of JS, and if we define a variable within a component/function, then that variable will be re-initialized every time we call the function.

```JAVASCRIPT
function App() {
  let count = 0;
  return {
    <p>{count}</p>
  }
}
```

- Back to `React.useState(0);`, we need to have some way, to access the current value of count, without redefining it in every function call.
- The `React.useState()` function is reaching into React, and pulling out the state variable.
- On the very first render, we are giving React the value, `const [count, setCount] = React.useState(0);` and putting it into the `count` variable.
- On the second render, we are reaching in and getting the new value.
- And so on, we are pulling the value out of React, which exist outside the scope of the function.

- The `setCount` function, JS doesn't give us a way to observe values, so we need to tell React it's time to render.
- The `setCount` is doing two things:
  - Primary thing, we are changing the value, and scheduling the update, so when we re-render the function, the `count` variable will have the new value.
  - Secondary thing, we are also signaling to React it's time to re-render.

- The easiest way to think about React, is a series of snapshots over time.
- When you first call a component, `App()` you get an initial value.
- Then every time you call the component after that, you get another snapshot and on and one.
- For each of these snapshot, the `state` variable is being populated within React.

- React doesn't compile to some other language, it is always restricted by the rules of JS.

### Forms

- Everyone's favorite thing to build, you know, the worlds most popular website, is just one form.
- Lots of packages claiming to solve all the form problems, but forms are not that bad to build in React.

#### Data Binding

- When building we apps, we want to sync a bit of state to a particular form input.
- For example, a 'username' field should be bound to the value of a `username` state variable.

- This is commonly known as 'data binding'. Most front end frameworks offer a way to bind a particular bit of state to a particular form control.
- Here is what it typically looks like in React.

```JAVASCRIPT
import React from 'react';

function SearchForm() {
  const [searchTerm, setSearchTerm] = React.useState('dogs');
  
  return (
    <>
      <form>
        <label htmlFor="search-input">
          Search:
        </label>
        <input
          type="text"
          id="search-input"
          value={searchTerm}
          onChange={(event) => {
            setSearchTerm(event.target.value);
          }}
        />
      </form>
      <p>
        Search term: {searchTerm}
      </p>
    </>
  );
}

export default SearchForm;
```

- What's going on above?
- If you start without the `value` or `onChange` you have an uncontrolled element.
- Uncontrolled Element, React creates the element, puts it on the page but after that, React doesn't know what we are doing with it.

- If you want to bind the input so that it always holds the value in state variable, `searchTerm`, you have to do a few things.
- First, you have to set the `value` property, `value="hello"`

```JAVASCRIPT
        <input
          type="text"
          id="search-input"
          value="hello"
        />
```

- However, notice you cannot change the value in the field, React is restricting you from updating.
- The reason, value servers as a form of lock, once you set the value, that input is always **bound** to that value.
- Now, we are going to add the expression slot, `value={searchTerm}`, but notice you still cannot change it.

```JAVASCRIPT
        <input
          type="text"
          id="search-input"
          value={searchTerm}
        />
```

- The input is still locked into whatever the value of the variable is, `searchTerm`.
- Why? This is because we haven't called the state function, `setSearchTerm`.

- Data binding example, set up an `onClick` button, to update the value of the state variable, `searchTerm`

```JAVASCRIPT
  return (
    <>
      <form>
        <label htmlFor="search-input">
          Search:
        </label>
        <input
          type="text"
          id="search-input"
          value={searchTerm}
        />
      </form>
      <p>
        Search term: {searchTerm}
      </p>

      <button
        onClick={() => setSearchTerm(Math.random())}
      >Click Me</button>
    </>
  );
```

- Now, when you click the button, the state variable updates and you can see the new value in the search input.
- But this is only **one way data binding**. Half of the equation.
- To set up **two way data binding**, you also want to update the value of state variable when you update the value of the search input.
- To do that, you have to add a `onChange` listener on the input.

```JAVASCRIPT
        <input
          type="text"
          id="search-input"
          value={searchTerm}
          onChange={(event) => {
            console.log(event);
          }}
        />
```

- And pass in the `event`, in JS, an event is an object that describes the change that just happened. For example, if you type in an input, the `change` event, describes that change.

- When you console log the event, you get back and object, the thing to pay attention to, is the `target`. Then you can get access to all the attributes of that `target`.
- For example; `console.log(event.target.value)`

```JAVASCRIPT
        <input
          type="text"
          id="search-input"
          value={searchTerm}
          onChange={(event) => {
            console.log(event.target.value);
          }}
        />
```

- Now when you type in the field, you see characters you are typing in.
- 🤔 But why isn't it showing in the field, and only logging in the console?
- React is allowing the `onChange` to fire, but as soon as it finishes firing the `onChange` event, React makes sure the value remains locked into what the value of `searchTerm` is.
- It happens so fast, **inbetween paint cycles**, you don't see it in render, but for brief moment, the value is updated and then React sets it back.

- Now, we can correct this by calling the `setSearchTerm` state function when the user types in the input search box.
- And going to call it with the `event.target.value`.

```JAVASCRIPT
        <input
          type="text"
          id="search-input"
          value={searchTerm}
          onChange={(event) => {
            setSearchTerm(event.target.value);
          }}
        />
```

- In other words, I type in the text box, the event fires, it references the element that triggered the event, and that shows the value the user just entered.
- And because you are updating the state variable `searchTerm`, and react locks it down, you have updated the `value={searchTerm}` to what the user put in the field.

- The shown value of the input, will always be in sync with whatever the `searchTerm` variable holds.

- Do you actually have to bind the value? Two way data binding, if you remove the `value` on the input, you loose two way data binding.
- If the state changes by some other way, say a button, the state variable will change, but it gets out of sync with what is shown in the input.

#### Controlled vs. Uncontrolled Inputs

- When we set the `value` attribute ona form control, we tell React that it should be a **controlled element**.
- In React, this means React is managing the input.

- If we don't set the `value`, the input is an **uncontrolled element**. This means React doesn't do any management.

- The golden rule: **An element should always be either a controlled or uncontrolled element.**
- This can lead to a common issue, like the following:

```JAVASCRIPT
import React from 'react';

function SignupForm() {
  // No default value:
  const [username, setUsername] = React.useState();
  
  return (
    <form>
      <label htmlFor="username">
        Select a username:
      </label>
      <input
        type="text"
        id="username"
        value={username}
        onChange={event => {
          setUsername(event.target.value);
        }}
      />
    </form>
  );
}

export default SignupForm;
```

- If you try typing into the input, you can an error in the console.
- `Warning: A component is changing an uncontrolled input to be controlled.`
- Why is this happening? We are setting the `value={username}`.
- **Here is the problem:** `username` is undefined at first, since there is not default value in the state hook. Here is what it's doing.

```JAVASCRIPT
const username = undefined;

<input
  type="text"
  id="username"
  value={username}
  onChange={event => {
    setUsername(event.target.value);
  }}
/>
```

- When we set `value` to `undefined`, the same as not setting it as all. react will treat the input as an **uncontrolled element**.
- When the user starts typing in the input, the `onChange` event updates the value of `username` from `undefined` to a string.
- And so, React flips the element to a controlled element, and throws the warning.

- **Here is how to solve the problem:** We always want to make sure we are passing a define `value`. You can do this by initializing `username` to an empty strong:

```JAVASCRIPT
// 🚫 Incorrect. `username` will flip from `undefined` to a string:
const [username, setUsername] = React.useState();

// ✅ Correct. `username` will always be a string:
const [username, setUsername] = React.useState('');
```

- With this change, our input is being controlled by React state from the very first render, since we are always passing a defined value.
- Even though empty strings are considered falsy they still count.

#### The onClick Parable

- In the context of a modern React application, this isn't usually what we want. We don't want to reload the entire page, we want to fetch a bit of data and re-render a few components with that data. This produces a faster, smoother user experience.

- A mistake most React dev's make, imagine this lesson, you are building a search form:

```JAVASCRIPT
import React from 'react';

function SearchForm({ runSearch }) {
  const [searchTerm, setSearchTerm] = React.useState('');
  
  return (
    <div className="search-form">
      <input
        type="text"
        value={searchTerm}
        onChange={event => {
          setSearchTerm(event.target.value);
        }}
      />
      <button>
        Search!
      </button>
    </div>
  );
}

export default SearchForm;
```

- In this example, `runSearch` is the function we want to call when the suer clicks on the Search button. In a real app, it would make s network request, and update some state with the results.

- Here is the question: how should we use this function?
- A lot of engineers will solve for this by adding an `onClick` handler to the submit button:

```JAVASCRIPT
<button onClick={() => runSearch(searchTerm)}>
  Search!
</button>
```

- There are a number ro problems with this approach.
- For example, what if the user tries to search by pressing "Enter" after typing in the text input?
- You could start to do down the path, and put an `onKeyDown` event listener?

```JAVASCRIPT
<input
  type="text"
  value={searchTerm}
  onChange={event => {
    setSearchTerm(event.target.value);
  }}
  onKeyDown={event => {
    if (event.key === 'Enter') {
      runSearch(searchTerm);
    }
  }}
/>
```

- **We are going down the wrong path here.** We are re-implementing stuff that the browser already knows how to do!

#### Use a Form

- The path you want to head down, to solve this problem, is to wrap your form controls in a `<form>` tag.

- Then, instead of listening for clicks and keys, we can listen for the **form submit event.**
- See how much simpler the code gets:

```JAVASCRIPT
import React from 'react';

function SearchForm({ runSearch }) {
  const [searchTerm, setSearchTerm] = React.useState('');
  
  return (
    <form
      className="search-form"
      onSubmit={event => {
        event.preventDefault();
        runSearch(searchTerm);
      }}
    >
      <input
        type="text"
        value={searchTerm}
        onChange={event => {
          setSearchTerm(event.target.value);
        }}
      />
      <button>
        Search!
      </button>
    </form>
  );
}

export default SearchForm;
```

- The form submit event will be called automatically when the suer clicks the button, or presses "Enter" whenever the input or button is focused.
- When that event fires, we will run our search.

- Instead of trying to re-create a bunch of standard web platform stuff, **we should use the platform native behavior** and let it solve these sorts of problems for us!

- By using a form submit event, we get to user client-side validation:

```JAVASCRIPT
<input
  type="password"
  required={true}
  minLength={8}
/>
```

#### Default form behavior

- One little quick with using `onSubmit`. We need to prevent teh default submission behavior:

```JAVASCRIPT
<form
  className="search-form"
  onSubmit={event => {
    event.preventDefault();
    runSearch(searchTerm);
  }}
>
```

- To understand why, remember back in the day, before `fetch`, `XMLHttpRequest` and JSON.
- If you wanted to make a request to a server, like when fetching search results, you couldn't request only the data. You needed to request a whole new HTML file.
- The user would be redirected to a new URL, and the sever would render a template into an HTML doc, using the data sent with the request.

- **Forms still operate this way by default.** When you submit a form, the browser will try to send the user to the URL specified by the `action` attribute:

```HTML
<!--
  Submitting this form will redirect the user to the
  /search page, sending along the data collected from
  the form fields.
-->
<form
  method="POST"
  action="/search"
>
```

- If we omit the `action` attribute, the browser will use the current URL, effectively reloading the page.

- 👀**In the context of a modern Rect app, this isn't what we want. We do not want to reload the entire page, we want to fetch a bit of data and re-render a few components with that data. This produces a faster, smoother user experience.**👀

- That's why we need to include `event.preventDefault()`. It stops the browser from executing a full page reload.

### Other Form Controls

- The web platform provides lots of additional form controls:
- Textareas
- Radio Buttons
- Checkboxes
- Selects
- Ranges
- Color pickers

- And are are a little different from one another, can be frustrating to work with.
- Take textarea for example, defines the value as children rather than a `value`

```HTML
<textarea>
  This is the current value
</textarea>
```

- Another example, the select tag uses a `selected` prop on one of the `<option>` children to signify the selected value:

```HTML
<select>
  <option value="a">
    Option 1
  </option>
  <option value="b" selected>
    Option 2
  </option>
  <option value="c">
    Option 3
  </option>
</select>
```

- Good news, React has tweaked many of these form controls so they have similar behavior. Lot less chaos with form controls in Rect.

- Essentially all form controls follow the same pattern:

1. The current value is tracked using either `value` (for most inputs) or `checked` (for checkboxes and radio).
2. We respond to changes with the `onChange` event listener.

- 🎁 [Bonus cheat sheet covering all the forms](https://courses.joshwcomeau.com/joy-of-react/02-state/11-bonus-cheatsheet)

#### Select Tag

- The `<select>` tag allows the user to select a single option form a list of predefined options.

- When working with select tags in React, they work pretty much exactly like text inputs. We use `value` and `onChange`. Example:

```JAVASCRIPT
import React from 'react';

function App() {
  const [
    selectedOption,
    setSelectedOption
  ] = React.useState('red');

  return (
    <form>
      <fieldset>
        <legend>
          What is your favorite color?
        </legend>
        
        <select
          value={selectedOption}
          onChange={event => {
            setSelectedOption(event.target.value)
          }}
        >
          <option value="red">
            Red
          </option>
          <option value="green">
            Green
          </option>
          <option value="blue">
            Blue
          </option>
        </select>
      </fieldset>
      
      <p>
        Selected value:
        <br />
        {selectedOption}
      </p>
    </form>
  );
}

export default App;
```

- By setting the `value` prop, we make this a controlled component, binding the selected option to our React state.
- When we add the `onChange` event listener, we allow this state to be changed by selecting a different option form the list.

- This feels like a huge improvement, compared to default functionality of this form control. We don't ahe to mess with adding the `selected` attribute to one of the `<option>` children.

#### Radio Buttons

- Radio buttons, allow the user to select one choice from a group of options.
- The tricky thing, is state is split across multiple independent HTML elements.
- There is only one `<select>` tag, but multiple `<input type="radio">` buttons.
- An example of a controlled radio button group in React:

```JAVASCRIPT
import React from 'react';

function App() {
  const [
    value,
    setValue
  ] = React.useState('no');

  return (
    <form>
      <fieldset>
        <legend>Do you agree?</legend>
        
        <input
          type="radio"
          name="agreed-to-terms"
          id="agreed-yes"
          value="yes"
          checked={value === "yes"}
          onChange={event => {
            setValue(event.target.value)
          }}
        />
        {' '}
        <label htmlFor="agreed-yes">
          Yes
        </label>
        <br />
        
        <input
          type="radio"
          name="agreed-to-terms"
          id="agreed-no"
          value="no"
          checked={value === "no"}
          onChange={event => {
            setValue(event.target.value)
          }}
        />
        {' '}
        <label htmlFor="agreed-no">
          No
        </label>
      </fieldset>
    </form>
  );
}

export default App;
```

- Radio has a ton of properties.
- `name` - The browser needs to know that each button is part of the same group, so selecting one option will de-select the others. This is done with the `name` prop. Each radio button ina group should share the same `name`.
- `value` - Each radio button has its own value. This property will be copied over to our React state when the option is ticked. This is the definition / meaning for each radio button.
- `id` - like other form controls, this is needed so that the `<label>` can be associated with he right input, so that clicking the label focuses the input.
- `checked` - This is the prop that binds a given radio button to our React state, making it a controlled value. It should be set to a boolean value: `true` if it's ticked, `false` if it is not. Only one radio button should be set to `true` at a time.

- In most cases we bind React state to `value`. In this case, we don't have a single `value` prop to bind to, since we have multiple radio buttons.
- So instead, we bind to `checked`, controlling the ticked/unticked status for each button in the group with React state.

### Exercises, Forms

- Form exercises

#### Controlled Country Picker

- AC's
- Use the `COUNTRIES` constant to dynamically generate a th set of `<option>` elements.
  - In order to map over an object,t you will need to use  something like Object.keys() or Object.entries()
- There should be a 'blank' option, selected by default. It shouldn't default to the first country in the list.
- The indicator at the bottom should update when the user changes their selected country.
- No warnings in the dev console.

```JAVASCRIPT
// starter code
import React from 'react';

import { COUNTRIES } from './data';

/*
  “COUNTRIES” is a dictionary-like object
  with the following shape:

  {
    AF: "Afghanistan",
    AL: "Albania",
    DZ: "Algeria",
  }
*/

function App() {
  const [
    country,
    setCountry,
  ] = React.useState('');

  return (
    <form>
      <fieldset>
        <legend>Shipping Info</legend>
        <label htmlFor="country">
          Country:
        </label>
        <select
          id="country"
          name="country"
        >
          {/* TODO: Options here! */}
        </select>
      </fieldset>

      <p className="country-display">
        Selected country: {country}
      </p>

      <button>Submit</button>
    </form>
  );
}

export default App;
```

- Use `Object.entries(COUNTRIES)` to get the data.
- `countryNames` is an array of arrays. Every array inside has exactly two items. First one is the ID, key of the object, and the second item is the value.
- The ID, keys are the unique identifier for each country. This is what we are going to use in our React state.
- Then you can use that with `map()` to iterate over all the countries.
- And use destructuring, to get variables the array.
- Remember, `countryName` is an array of an array, and every array has exactly two items. `['US', 'United States']`
- First item is going to be the country code, `'CA'`
- Second item is the country label, like `'Canada'`

```JAVASCRIPT
// ['CA', 'Canada'], you can destructure that data and use it in your function.
countryNames.map(([id, label]) => {
  return (
    <option>
      {label}
    </option>
  )
})
```

- Now the countries will be rendered out in the select.
- Also on the `<option>`, add the `value={id}` and use that as the key, `key={id}`.
- Now I want to use the `id` as the value, since that will be set in react state. `value={id}`
- And since the `id` is unique, you can use that as the `key`, `key={id}`.

- Event, add the `onChange` handler to the select.

```JAVASCRIPT
onChange={event => {
  console.log(event.target);
}}
```

- And to get the value, just update to `event.target.value`
- Then take that value and put it into your state setter, `setCountry()` function.

```JAVASCRIPT
onChange={event => {
  setCountry(event.target.value);
}}
```

- Now you see the selected country update in the UI, because we are passing it along to `react.state`

- Now we have to do the data binding.
- So you have two way data binding, and if you want to pre-populate the state. `value={country}`

```JAVASCRIPT
  <select
    id="country"
    name="country"
    value={country}
    onChange={event => {
      setCountry(event.target.value);
    }}
  >
```

- Setting an initial value, empty string, `React.useState('')`
- In the `select` add an option for the empty / default

```JAVASCRIPT
   <select
     required
     id="country"
     name="country"
     value={country}
     onChange={event => {
       setCountry(event.target.value);
     }}
   >
     {/* TODO: Options here! */}
     <option value="">-- Select Country --</option>
     {countryNames.map(([id, label]) => {
        return (
          <option value={id} key={id}>
            {label}
          </option>
        )
     }
     )}
   </select>
```

- One nice thing, when you set the `value=""` to empty, and add the `required` property to the `select`, you get some built in validation.

- Nice UX enhancement, to add a `<optgroup label="Countries"></optgroup>` around the options, makes a nice nested feel to the countries list.
- The last thing, is to display the full name of the country.

```JAVASCRIPT
  <p className="country-display">
    Selected country: {COUNTRIES[country]}
  </p>
```

- Full solution:

```JAVASCRIPT
import React from 'react';

import { COUNTRIES } from './data';

/*
  “COUNTRIES” is a dictionary-like object
  with the following shape:

  {
    AF: "Afghanistan",
    AL: "Albania",
    DZ: "Algeria",
  }
*/

const countryNames = Object.entries(COUNTRIES);
// console.log(countryNames);

function App() {
  const [
    country,
    setCountry,
  ] = React.useState('');

  return (
    <form>
      <fieldset>
        <legend>Shipping Info</legend>
        <label htmlFor="country">
          Country:
        </label>
        <select
          required
          id="country"
          name="country"
          value={country}
          onChange={event => {
            setCountry(event.target.value);
          }}
        >
          <option value="">-- Select Country --</option>
          <optgroup label="Countries">
            {countryNames.map(([id, label]) => {
               return (
                 <option value={id} key={id}>
                   {label}
                 </option>
               )
            }
            )}
          </optgroup>

        </select>
      </fieldset>

      <p className="country-display">
        Selected country: {COUNTRIES[country]}
      </p>

      <button>Submit</button>
    </form>
  );
}

export default App;
```

### Exercise, Two Factor Authentication

- Two factor authentication is teh best practice in terms of secure sign in. The most common pattern is to prompt the user to enter a short code from the app.

- Let's build this form.
- AC's
  - The input value should be held in React state.
  - When the user submits their code, a `window.alert` should let them know whether it's correct or not, by comparing their submitted value with teh `CORRECT_CODE` constant.
  - A `<form>` tag should dbe used.

- Hint: You need to mange default behavior.

```JAVASCRIPT
// starter code
import React from 'react';

const CORRECT_CODE = '123456';

function TwoFactor() {
  return (
    <>
      <label htmlFor="auth-code">
        Enter authorization code:
      </label>
      <div className="row">
        <input
          id="auth-code"
          type="text"
          required={true}
          maxLength={6}
        />
        <button>Validate</button>
      </div>
    </>
  );
}

export default TwoFactor;
```

- **My attempt:**
- AC's
- The input value should be held in React state.

```JAVASCRIPT
  const [code, setCode] = React.useState('');
```

- When the user submits their code,a `window.alert` should let them know whether it's correct or not, by comparing their submitted value with the `CORRRECT_CODE` constant.

```JAVASCRIPT
    if (code === CORRECT_CODE) {
      window.alert('correct');
    } else 
      window.alert('please try again');

    // you could also do this with ternary
    code ===  CORRECT_CODE ? window.alert('correct') : window.alert('please try again');
```

- A `<form>` tag should be used and manage the default behavior

```JAVASCRIPT
    <form
      onSubmit={event => {
        event.preventDefault();
        code ===  CORRECT_CODE ? window.alert('correct') : window.alert('please try again');
      }}
    >
```

```JAVASCRIPT
import React from 'react';

const CORRECT_CODE = '123456';

function TwoFactor() {

  const [code, setCode] = React.useState('');
  
  return (
    <form
      onSubmit={event => {
        event.preventDefault();
          if (code === CORRECT_CODE) {
            window.alert('correct')
          } else 
            window.alert('please try again');
      }}
    >
      <label htmlFor="auth-code">
        Enter authorization code:
      </label>
      <div className="row">
        <input
          id="auth-code"
          type="text"
          required={true}
          maxLength={6}
          value={code}
          onChange={(event) => {
            setCode(event.target.value);
            console.log(code);
          }}
        />
        <button>Validate</button>
      </div>
    </form>
  );
}

export default TwoFactor;
```

- Josh's solution:
- Creates a separate function for the submit.

```JAVASCRIPT
  function handleSubmit(event) {
    // prevent the default behavior browser refresh
    event.preventDefault();
    // Creates a boolean value
    const isCorrect = code === CORRECT_CODE;
    window.alert(isCorrect ? 'Correct!' : 'Incorrect');
  }

  return (
    <form onSubmit={handleSubmit}>
    // rest of stuff here
    </form>
  )
```

### Exercise, Generative Art

- Build some generative art in this exercise.
- Wire up the form controls to the Rect state so when update the forms, they will tweak the art.

- AC's

1. The range slider should be bound to the `numOfLines` state.
2. The select control should be bound to the `colorTheme` state.
3. The radio buttons should be bound to the `shape` state.
4. The radio button labels should work correctly. The user should be able to click the text "Polygons" to select that option.
5. The inputs should conform to the HTML standards, eg. radio buttons should be grouped using the "name attribute.

- Note: All change should happen in the `App.js.`

- My attempt:

1. The range slider should be bound to the `numOfLines` state.

```JAVASCRIPT
// React state: const [numOfLines, setNumOfLines] = React.useState(5);
    <label
      htmlFor="num-of-lines"
      className="control-heading"
    >
      Number of Lines:
    </label>
    <input
      id="num-of-lines"
      type="range"
      min="1"
      max="15"
      value={numOfLines}
      onChange={(element) => {
        return (
          setNumOfLines(element.target.value)
        )
      }}
    />
```

2. The select control should be bound to the `colorTheme` state.

```JAVASCRIPT
// React state: const [colorTheme, setColorTheme] = React.useState('basic');
    <label
      className="control-heading"
      htmlFor="color-theme"
    >
      Color Theme:
    </label>
    <select 
      id="color-theme"
      value={colorTheme}
      onChange={(element) => {
        return (
          setColorTheme(element.target.value)
        )
      }}
    >
      <option value="basic">
        Basic
      </option>
      <option value="monochrome">
        Monochrome
      </option>
      <option value="fruit-loops">
        Fruit Loops
      </option>
      <option value="spooky">
        Spooky Night
      </option>
    </select>
```

3. The radio buttons should be bound to the `shape` state.

- For radio buttons, use the `checked` property to manage the state. We are saying that this input should be checked if a certain condition is true. Add a condition on the `checked` property. `checked={shape === "circles}` then this input/radio should be checked.

```JAVASCRIPT
    <div className="radio-wrapper">
      <div className="radio-option">
        <input 
          type="radio" 
          name="choose-shape"
          id="shape-circles"
          value="circles"
          checked={shape === "circles"}
          onChange={(element) => {
            return (
              setShape(element.target.value)
            )
          }}
        />
        <label htmlFor="shape-circles">
          Circles
        </label>
      </div>
      <div className="radio-option">
        <input 
          type="radio"
          name="choose-shape"
          id="shape-polygons"
          value="polygons"
          checked={shape === "polygons"}
          onChange={(element) => {
            return (
              setShape(element.target.value)
            )
          }}
        />
        <label htmlFor="shape-polygons">
          Polygons
        </label>
      </div>
    </div>
```

1. The radio button labels should work correctly. The user should be able to click the text "Polygons" to select that option.

- Radio buttons, to make them part of the same group, add the `name="my-radio` property to all the radio button in the same group.
- To make the label interactive so you can click on it. Give the radio button input an `id="shape-circle"` and then give the corresponding label the same value, but use the `htmlFor="shape-circle"` attribute.

```JAVASCRIPT
    <div className="radio-option">
      <input 
        type="radio"
        name="choose-shape"
        id="shape-polygons"
        value="polygons"
        checked={shape === "polygons"}
        onChange={(element) => {
          return (
            setShape(element.target.value)
          )
        }}
      />
      <label htmlFor="shape-polygons">
        Polygons
      </label>
    </div>
```

  Note: Do things in a conventional DOM friendly way, and you will sidestep a whole lot of issues from server side rendering. If React does not load for example, the browser can still perform the functionality.

## Prop Vs. State

- When learning React it is normal for the concepts of "props" and "state" to be a bit confusing and intermingled.
- What is the difference between them exactly? When do you use props and when to use state?

### Props

- "Props is short for "properties". At the micro level, the are like the attributes we place on HTML elements, like `class` or `href`:

```JAVASCRIPT
  <a
    class="nav-link"
    href="/category/stuff"
  >
```

- For example, the `Button` component below takes a "variant" prop. This prop will be used internally to control styling, like how the `class` attribute works in HTML.

```JAVASCRIPT
    import Button from './Button';

    function App() {
      return (
        <div className="box">
          <p>
            Are you sure you want to continue?
          </p>
          <div className="actions">
            <Button variant="secondary">
              Cancel
            </Button>
            <Button variant="primary">
              Confirm
            </Button>
          </div>
        </div>
      );
    }

    export default App;
```

- Props allow us to customize behavior of a given component, so the exact same component can do different things in different scenarios.

- 🚀 **Props are the inputs to our components, like arguments passed to a function.**
  
### State

- In the example above, our application is static. every time we run this code, we get the same results.
- But what if we wanted stuff to change over time? **That's where state comes in.**

- Let's tweak our example:

```JAVASCRIPT
    import React from 'react';

    import Button from './Button';

    function App() {
      const [hasAgreed, setHasAgreed] = React.useState(false);
      
      return (
        <div className="box">
          <p>
            Are you sure you want to continue?
          </p>
          <label htmlFor="confirm-checkbox">
            <span className="required">*</span>
            <input
              id="confirm-checkbox"
              type="checkbox"
              value={hasAgreed}
              onChange={() => setHasAgreed(!hasAgreed)}
            />
            <span>
              I agree with <a href="/terms">the terms</a>.
            </span>
          </label>
          <div className="actions">
            <Button
              variant="secondary"
              isEnabled={true}
            >
              Cancel
            </Button>
            <Button
              variant="primary"
              isEnabled={hasAgreed}
            >
              Confirm
            </Button>
          </div>
        </div>
      );
    }

    export default App;
```

- We have a new bit of state, `hasAgreed`, and we need to use that data to power our "confirm" button.
- This reveals an important truth about props: **they are tunnels that allow data to flow through our application.**

- Let's dig in, video
- When you create a state variable, it is only available within the component. It's not global or available to other components.
- Two ways to think about it:
  - Props are used to control the configuration of a particular instance of a component.
  - Props are also, the tracks we use to funnel data from one place to another. **The way, parent components communicate to children components.** They are the tunnels that connect our different components.

- 🤔 The data that we can use in our props, can be a string, boolean, or we can use react state.
- We use a prop, to pass a bit of state to one place to another.

- Props are basically a subway network, a system of tunnels we can use to funnel data around.

## Complex State

- So far we have seen how store numbers and strings in React state. Most times, our state will be in a more complex shape, ike an object or an array.

- For example; in the previous exercise, the gradient generator tool where users select 2 to 5 colors, and use state to generate a gradient.
- How to you keep track of the state for the colors?

- In this case, the `colors` state variable, hold an array of strings:

```JAVASCRIPT
  const [colors, setColors] = React.useState(['#FFD500', '#FF0040']);
```

- React doesn't care what type out state is. We can store numbers or strings or arrays or objects. We can even store functions in state if we want!

- 👀 But there's a catch, React state changes have to be immutable.
- When we call `setColors`, we need to provide a brand new array.
- **We should never mutate arrays or objects help in state**

- Immutability is a big topic in JS. You can see the lesson in the JS primer, [Assignment vs. Mutation](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/11-assignment-vs-mutation)

### Mutation Bugs

- Video summary:
- In our color picker example, we are unable to choose the colors.
- The problem, when we use complex state, with arrays or objects, we have to specify a new array.
- Otherwise, React doesn't think we changed anything.
**- React uses, reference equality as a way to check if something has changed.**
- React will check, is the object or array you are passing me, the same entity, the same reference, if it is, React does not re-render. React figures it is intentional.

- Refresher: equality, objects, memory, pointers, really important to have a firm grasp on these concepts.
- For this exercise, startup a node console to explore these concepts. New terminal, `node`

- Create a new array

```JAVASCRIPT
  let arr = [1, 2, 3, 4]
```

- Then create a new variable called `nextArr` and assign it to `arr`

```JAVASCRIPT
  let nextArr = arr;
```

- Then, take `nextArr` and modify the first value to be equal to 5

```JAVASCRIPT
  nextArr[0] = 5
```

- Now, the question is, what is the value of `arr`? You might think, the value is the same, because you didn't change `arr`, you changed `nextArr`
- But when you check the value of `arr`, is has the new initial value of 5. `[5, 2, 3, 4]`
- In fact, `nextArr === arr` is `true`, exactly equal to each other.
- What is going on here in JS? We don't have to think about the high level memory storage. In JS, whenever we write a variable, it makes a physical change in the computer, stores it in RAM.
- And the variable is just a pointer to that.

- For example; our `arr` variable, just points to this array held in memory.
- And when you say, `let nextArr = arr;`, we are not creating a new array, you are just pointing `arr` and `nextArr` to the same thing, same array.
- Both variables point to the same entity that exist in memory.
- When we change an item in `nextArr`, we are changing the same item, we are not creating a clone of it.

- If you take two arrays, with the exact same value, are they equal?

```JAVASCRIPT
  [1, 2, 3] === [1, 2, 3]
  // false
```

- This is `false` because you have two separate arrays.
**- The way JS works, you have objects in memory, and variables that point to them**

- What if you wanted to create a copy with the same values?
- You can use the spread operator `...`, this will copy the contents of the array.

```JAVASCRIPT
  let betterCloneArr = [...arr]
  // [ 5, 2, 3, 4 ]
```

- This works not matter how many values you have, it is unpacking the array into the new `betterCloneArr` variable.
- And it's creating a new array
- Now notice, `betterCloneArr` is not equal to `arr`

```JAVASCRIPT
  betterCloneArr === arr
  // false
```

- 👀 JS Primer on the [Spread operator](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/12-rest-spread)
- And a good blog post, [visual reference to JS](https://daveceddia.com/javascript-references/)

- In our color picker example, all that matters is that React gets a new object and does the re-render.

### Never mutate React state (even when it seems to work)

- In the example, we follow these steps

1. Create a new array
2. Modify that new array
3. Set the new array into state

- Is it OK to flip the order of the first two steps? What is we modify the array, then clone it? Like this:

```JAVASCRIPT
  <input
  onChange={event => {
    // Mutate the array:
    colors[index] = event.target.value;

    // Create a new copy, and set it into state:
    setColors([...colors]);
  }}
/>
```

- Seems simpler, right? Make whatever modifications we want, and then copy the array right before we call `setColors`, so that we are providing a new value.

- **Here is the problem**. By doing it this way, we are modifying the values held in React state.
- React really doesn't expect us to do this, and has no guardrails in place to warn us if we try.

- You might get lucky and get away with it once, but if you make a habit of it, you will likely start getting weird / glitchy behavior.
- A random part fo the page won't be updated when supposed to. Or maybe the DOM structure will get shuffled, putting elements in different parts of the DOM.

- These bugs are really hard to diagnose and fix, you will save yourself a whole lot of trouble if you do your best to avoid mutating React state.

### But isn't' this inefficient?  

- To create brand new arrays on every single change, isn't that inefficient? Wouldn't it be more performant to mutate the existing array instead?
- When it comes to React state, these is no choice; we need to produce a new array whenever the state changes.

- Fortunately, this isn't an issue. Cloning an array is an incredibly quick operation.

- In our color gradient picker example, when we clone the `[...colors]` array, it takes about 2 microseconds.
- That's two millionths of a second. For context, the average human blink is 100 milliseconds; we could clone 50,000 arrays in the blink of an eye.

### Exercises, Complex State

- Working on the gradient generator tool! Instead of limiting the user to 2 colors, let's ad up to 5 colors.

- AC's
  - The color inputs should work; picking a new color should update the gradient accordingly.
  - Clicking "Add color" should add a new color, at the end of the array.
  - Clicking "Remove color" should remove the last color in the array.
  - When adding new colors, they should default to `FF0000` bright red.
  - There should always be between 2 and 5 colors. No more, no less. The user should be given feedback as to why the buttons stop working once they hit a limit.

- Starter code, gradient picker

```JAVASCRIPT
// App.js
import React from 'react';
function App() {
  const [colors, setColors] = React.useState([
    '#FFD500',
    '#FF0040',
  ]);
  
  const colorStops = colors.join(', ');
  const backgroundImage = `linear-gradient(${colorStops})`;

  return (
    <div className="wrapper">
      <div className="actions">
        <button>
          Remove color
        </button>
        <button>
          Add color
        </button>
      </div>
      
      <div
        className="gradient-preview"
        style={{
          backgroundImage,
        }}
      />
      
      <div className="colors">
        {colors.map((color, index) => {
          const colorId = `color-${index}`;
          return (
            <div key={colorId} className="color-wrapper">
              <label htmlFor={colorId}>
                Color {index + 1}:
              </label>
              <div className="input-wrapper">
                <input
                  id={colorId}
                  type="color"
                  value={color}
                  onChange={(element) => {
                      // create a new array
                      const nextColors = [...colors];
                      // mutate it
                      nextColors[index] = event.target.value;
                      // add to state
                      setColors(nextColors);
                      console.log(colors);
                  }}
                />
              </div>
            </div>
          );
        })}
      </div>
    </div>
  );
}

export default App;
```

- AC: The color inputs should work; picking a new color should update the gradient accordingly.

```JAVASCRIPT
<div className="input-wrapper">
    <input
      id={colorId}
      type="color"
      // add a value to make a controlled
      value={color}
      onChange={(element) => {
          // create a new array
          const nextColors = [...colors];
          // mutate it
          nextColors[index] = event.target.value;
          // add to state
          setColors(nextColors);
          console.log(colors);
  />
</div>
```

- AC: Clicking "Add color" should add a new color, at the end of the array.
- Tip 🤔 Don't forget the rule about 'Never Mutate React State',
  - 1. Create a new array
  - 2. Modify that new array
  - 3. Set that new array into state

```JAVASCRIPT
   <button
     onClick={(event) => {
       console.log(event.target)
       // create a new array
       let newColors = [...colors]
       // modify that array, set default to red
       newColors.push("#FF0000")
       // set it to state
       setColors(newColors)
       console.log(colors)
     }}
   >
     Add color
   </button>
```

- AC: Clicking "Remove color" should remove the last color in the array. Don't forget about about the react mutate rule.

```JAVASCRIPT
  <button
    onClick={(event) => {
      console.log(event.target)
      // 1. create a new array
      let removeColor = [...colors]
      // 2. modify that array
      removeColor.pop()
      // 3. set to state
      setColors(removeColor)
      console.log(`new colors ${colors}`)
    }}
  >
    Remove color
  </button>
```

- AC: When adding new colors, they should default to `#FF000` (bright red).

```JAVASCRIPT
       // modify that array, set default to red
       newColors.push("#FF0000")
```

- AC: There should always be between 2 and 5 colors. No more, no less. The user should be given feedback as to why the buttons stop working once they hit a limit.

```JAVASCRIPT
// my attempt
// get the number or colors from the colors array
// if 2 colors, disable the remove button, throw an alert
// if 5 colors, disable the add button, throw an alert
```

```JAVASCRIPT
import React from 'react';
function App() {
  const [colors, setColors] = React.useState([
    '#FFD500',
    '#FF0040',
  ]);
  
  const colorStops = colors.join(', ');
  const backgroundImage = `linear-gradient(${colorStops})`;

  // function for the minimum
  function colorMin(param) {
    console.log(param)
    param < 2 ? alert('min of 2') : undefined
  }

  // function for the max
  function colorMax(param) {
    console.log(param)
    param > 5 ? alert('max of 5') : undefined
  }

  return (
    <div className="wrapper">
      <div className="actions">
        <button
          onClick={(event) => {
            console.log(event.target)
            // 1. create a new array
            let removeColor = [...colors]
            // 2. modify that array
            removeColor.pop()
            // 3. set to state
            setColors(removeColor)
            console.log(`new colors ${colors}`)
            // color min
            colorMin(removeColor.length)
          }}
        >
          Remove color
        </button>
        <button
          onClick={(event) => {
            console.log(event.target)
            // 1. create a new array
            let newColors = [...colors]
            // 2. modify that array
            newColors.push("#FF0000")
            // 3. set it to state
            setColors(newColors)
            // color max
            colorMax(newColors.length)
          }}
        >
          Add color
        </button>
      </div>
      
      <div
        className="gradient-preview"
        style={{
          backgroundImage,
        }}
      />
      
      <div className="colors">
        {colors.map((color, index) => {
          const colorId = `color-${index}`;
          return (
            <div key={colorId} className="color-wrapper">
              <label htmlFor={colorId}>
                Color {index + 1}:
              </label>
              <div className="input-wrapper">
                <input
                  id={colorId}
                  type="color"
                  // add a value to make a controlled
                  value={color}
                  onChange={(element) => {
                      // create a new array
                      const nextColors = [...colors];
                      // mutate it
                      nextColors[index] = event.target.value;
                      // add to state
                      setColors(nextColors);
                      console.log(colors);
                  }}
                />
              </div>
            </div>
          );
        })}
      </div>
    </div>
  );
}

export default App;
```

- Video Solution:
- For the condition AC, to check the minimum and max conditions
- Create a separate function for the `addColor` and `removeColor` with and if statement for the min, max requirement.

```JAVASCRIPT
  function addColor() {
    if (colors.length >=5) {
      window.alert('There is a maximum of 5 colors')
      return; //stops here
    }
     // 1. create a new array
     let newColors = [...colors]
     // 2. modify that array
     newColors.push("#FF0000")
     // 3. set it to state
     setColors(newColors)    
  }

  function removeColor() {
    if(colors.length <=2) {
      window.alert('There is a minimum of 2 colors')
      return; //stops here
    }
      // 1. create a new array
      let removeColor = [...colors]
      // 2. modify that array
      removeColor.pop()
      // 3. set to state
      setColors(removeColor)
  }
```

## Dynamic Key Generation

- When interating over data with `.map`, we nee dto give each Rect element a unique `key` attribute so that React knows which DOM operations to trigger between renders.

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

## Lifting State Up

- In React, lifting state up is one of the core React concepts, and really important. One of the most critical lessons in this course.

- Video notes:
- Data fetching, API request to the backend, to fetch all the information.
- In this example, a simple search UI, with an `<App/>` and two children components, `<SearchForm />`, and `<SearchResults />`

- `<SearchForm />` is the component that holds the actual state, `searchTerm`
- The state is defined in `<SearchForm />`

```JAVASCRIPT
function SearchForm() {
  const [searchTerm, setSearchTerm] = React.useState('');
}
```

- 🤔 Initially, you might think you can just push the state from one component to the other, two way, regardless of parent child relationship. Any part of the application could access any other part of the application, it's data or state.
- The problem is, over time you end up with spaghetti code. You end up with this situation where everything is pointing to everything.
- You get in a situation, where, can I change or remove this bit of state, what far area of the app will this impact or break?
- Very poor visibility in how these things are setup?

- 📣 📣 The rule in React, is that data can only pass through components via props, or via context. 📣 📣
- We have to follow the lines of our application.
- Have some data in `<App />` we would be able to pass some data down to it children, `<SearchForm />` and `<SearchResults />` through props.

```JAVASCRIPT
  // App.js
  function App() {
    return (
      <>
        <header>
          <a className="logo" href="/">
            Wanda’s Fruits
          </a>
          <SearchForm />
        </header>
        <main>
          <SearchResults />
        </main>
      </>
    );
  }
```

- The way we solve this problem, WE HAVE TO LIFT THE STATE UP, we have to lift it up higher in the tree.
- 🤔 As long as a parent can pass to a child, THAT'S HOW THE WHOLE DATA FLOW WORKS.
- This is also called, unidirectional data flow:
  - parents can pass to children
  - but children cannot pass to parents
  - and siblings cannot pass to siblings
- DATA ALWAYS FLOWS IN THIS SINGULAR DIRECTION

- In our search example, we are going to lift the state up, to the `<App/>` component.

```JAVASCRIPT
// App.js
import React from 'react';

import SearchForm from './SearchForm';
import SearchResults from './SearchResults';

function App() {
  // Lift the state up to App
  const [searchTerm, setSearchTerm] = React.useState('');
  // Pass it down below via props in each component
  return (
    <>
      <header>
        <a className="logo" href="/">
          Wanda’s Fruits
        </a>
        <SearchForm  searchTerm={searchTerm}/>
      </header>
      <main>
        <SearchResults searchTerm={searchTerm}/>
      </main>
    </>
  );
}

export default App;
```

- Then add a new prop, to the child components, `<SearchForm />` and `<SearchResults />`

```JAVASCRIPT
// SearchForm.js
import React from 'react';

// pass in the prop searchTerm
function SearchForm({ searchTerm }) {
  
  function runSearch(event) {
    event.preventDefault();
    
    // Actual search stuff omitted from
    // this example.
  }
  
  return (
    <form onSubmit={runSearch}>
      <label
        className="visually-hidden"
        htmlFor="search-input"
      >
        Search term:
      </label>
      <input
        id="search-input"
        className="search-input"
        // bind to the form
        value={searchTerm}
        onChange={event => {
          setSearchTerm(event.target.value);
        }}
      />
      <button>
        Search
      </button>
    </form>
  );
}

export default SearchForm;
```

```JAVASCRIPT
// SearchResults.js
function SearchResults({ searchTerm }) {
  return (
    <p>
      You searched for: {searchTerm}.
    </p>
  );
}

export default SearchResults;
```

- Now if you set the default value in the `useState()` in the parent component `<App />` you will see it passed down to the children components.
- We passed down a static value, but how do you update this state? When someone searches in the form, how do you update the state now.

- 🤔 How do you write to state that lives in a different component? 🤔
- Functions in JS, is data just like any other data, JS is a functional programming language.
- 🚀 You can just pass down the state function, `setSearchTerm` down as a prop, so the children have access to it, you can pass down a function as a prop, just like any other data. And that child component will have access to it.

```JAVASCRIPT
// App.js, pass down the setSearchTerm state function
    <SearchForm  
      searchTerm={searchTerm}
      setSearchTerm={setSearchTerm}
    />
```

- Then in your child component, pass in the `setSearchTerm` function, so the form can be updated.

```JAVASCRIPT
// SearchForm.js
function SearchForm({ searchTerm, setSearchTerm }) {
  // component here
}
```

- Step by Step Review, walk though what happens when you make a single change to our search input.

- Type the letter `a`. The first thing that happens is the `onChange` event fires on the `<SearchForm />` component.
- The `onChange` event, evokes the `setSearchTerm` function, passing in the new value, which is the letter `a`, grabbing it from `event.target.value`.

```JAVASCRIPT
  onChange={event => {
    setSearchTerm(event.target.value);
  }}
```

- Whenever you call a state setter function in react, it is going to re-render that component that owns that bit of state.
- Because the `setSearchTerm` function is owned by `<App/>` the app component re-renders. Whenever their is a change in the state, `searchTerm`, the component re-renders.
- So when `searchTerm` is updates in state, it tells react to update the component, and any children components.
- So `<App/>` updates and forces a re-render for `<SearchForm/>` and `<SearchResults/>`

```JAVASCRIPT
function App() {
  
  const [searchTerm, setSearchTerm] = React.useState('');
  
  return (
    <>
      <header>
        <a className="logo" href="/">
          Wanda’s Fruits
        </a>
        <SearchForm  
          searchTerm={searchTerm}
          setSearchTerm={setSearchTerm}
        />
      </header>
      <main>
        <SearchResults searchTerm={searchTerm}/>
      </main>
    </>
  );
}
```

- So when `<SearchResults />` re-renders, it is now equal to the search term, `a`

- Now, in a larger application, where do you lift the state to?
- You might think, I can just lift up the state, all the way up to `<App />`, that way every single component would have access if needed.
- But not a best practice, a couple reasons
  - **Performance**, we only want to re-render the components that require the data. If you put the state to far up, you will be re-rendering components that don't care about that data.
  - **Code organization**, hard to manage that many state hooks in the same component, unique names for it all. A simple blog for example, could have hundreds of state hooks.
- 🤔 You should find the lowest ancestor, you want to move the state as low as you can but no lower.
- 🚀 This is our strategy, for lifting state up. 🚀

### Exercises, Lifting up State

- Exercises for lifing up state

#### Cookie Clicker

- We will build a simple version of the cookie clicker game, but instead we will use a Canadian toonie, whatever that is.

- AC's
  - "Your coin balance", at the bottom of the page, should update to show the value of `numOfCoins`.
  - Clicking the coin should increment this value by 2, since it's a $2 coin.

- Parent Component

```JAVASCRIPT
// App.js
import React from 'react';

import BigCoin from './BigCoin';

function App() {
  return (
    <div className="wrapper">
      <main>
        <BigCoin />
      </main>
      <footer>
        Your coin balance:
        <strong>0</strong>
      </footer>
    </div>
  );
}

export default App;
```

- The child component

```JAVASCRIPT
// BigCoin.js
import React from 'react';
import './BigCoin.css';

function BigCoin() {
  const [numOfCoins, setNumOfCoins] = React.useState(0);
  
  return (
    <div className="coin-wrapper">
      <button
        className="coin"
        onClick={() => setNumOfCoins(numOfCoins + 2)}
      >
        <span className="visually-hidden">
          Add 2 coins
        </span>
        <img
          className="coin-image"
          alt=""
          src="https://sandpack-bundler.vercel.app/img/toonie.png"
        />
      </button>
    </div>
  );
}

export default BigCoin;
```

- AC 1: "Your coin balance", at the bottom of the page, should update to show the value of `numOfCoins`.
  - Lift up the state, and pass it down to `<BigCoin/>` component
- AC 2: Pass in the state variable in the footer.

```JAVASCRIPT
import React from 'react';

import BigCoin from './BigCoin';

function App() {

  // lift up state
  const [numOfCoins, setNumOfCoins] = React.useState(0);
  
  return (
    <div className="wrapper">
      <main>
        <BigCoin 
          numOfCoins={numOfCoins}
          setNumOfCoins={setNumOfCoins}
        />
      </main>
      <footer>
        Your coin balance:
        <strong>{numOfCoins}</strong>
      </footer>
    </div>
  );
}

export default App;
```

- Pass down the state via props to the `<BigCoin />` component

```JAVASCRIPT
import React from 'react';
import './BigCoin.css';

function BigCoin({ numOfCoins, setNumOfCoins }) {
  
  return (
    <div className="coin-wrapper">
      <button
        className="coin"
        onClick={() => {
          setNumOfCoins(numOfCoins + 2)
          console.log(numOfCoins)
        }}
      >
        <span className="visually-hidden">
          Add 2 coins
        </span>
        <img
          className="coin-image"
          alt=""
          src="https://sandpack-bundler.vercel.app/img/toonie.png"
        />
      </button>
    </div>
  );
}

export default BigCoin;
```

#### Exercise, Shopping List

- Let's build our own shopping list, the design and JSX is already implemented, but it is entirely static.
- Your job is to update the code so that it works.

- AC's
- 1. The shown list of items should be driven from React state. We can remove the placeholder foods, and start wtih an empty list.
- 2. Submitting the form should add a new item to the list, and show it in the UI.
- 3. When submitting the form, the text input should be reset, so that it's empty. This way, users can easily add multiple items without having to erase their previous entry.
- 4. There should be no "key" warnings in the console. Ideally, you shouldn't use the index for the key.

- Starter code

```JAVASCRIPT
// App.js
import React from 'react';

import AddNewItemForm from './AddNewItemForm';

function App() {
  return (
    <div className="wrapper">
      <div className="list-wrapper">
        <ol className="shopping-list">
          <li>Avocados</li>
          <li>Broccoli</li>
          <li>Carrots</li>
        </ol>
      </div>
      <AddNewItemForm />
    </div>
  );
}

export default App;
```

- Child component

```JAVASCRIPT
// AddNewItemForm.js
import React from 'react';

function AddNewItemForm() {
  return (
    <div className="new-list-item-form">
      <form>
        <label htmlFor="new-list-form-input">
          New item:
        </label>
        
        <div className="row">
          <input
            id="new-list-form-input"
            type="text"
          />
          <button>
            Add
          </button>
        </div>
      </form>
    </div>
  );
}

export default AddNewItemForm;
```

- **AC 1.** Generate the list dynamically, based on state, map over the items and generate a unique ID for the key.

```JAVASCRIPT
// App.js
import React from 'react';

import AddNewItemForm from './AddNewItemForm';

function App() {
  // add state for the items, generate id
  const [items, setItems] = React.useState([
    { label: 'Apple', id: 123 },
    { label: 'Bananna', id: 456 },
  ]);
  
  return (
    <div className="wrapper">
      <div className="list-wrapper">
        <ol className="shopping-list">
          {items.map((item) => (
            <li key={item.id}>{item.label}</li>
          ))}
        </ol>
      </div>
      <AddNewItemForm />
    </div>
  );
}

export default App;
```

- How would you add something to the list? Create a function, that will produce one of the objects and dynamically come up with a unique id and put that into state.

```JAVASCRIPT
  function handleAddItem(label) {
    // create the new item and generate unique id
    const newItem = {
      label: label,
      id: Math.random(),
    };
    // create new array, pass in the existing items, and add the new ones
    const nextItems = [...items, newItem];
    // put that into state
    setItems(nextItems);
  }
```

- Next, we have to figure out how to call this function, `handleAddItem()` pass it down as a prop to the `<AddNewItemForm />` component.

```JAVASCRIPT
// App.js
import React from 'react';

import AddNewItemForm from './AddNewItemForm';

function App() {
  // add state for the items, generate id
  const [items, setItems] = React.useState([
    { label: 'Apple', id: 123 },
    { label: 'Bananna', id: 456 },
  ]);

  function handleAddItem(label) {
    // create the new item and generate unique id
    const newItem = {
      label: label,
      id: Math.random(),
    };
    // create new array, pass in the existing items, and add the new ones
    const nextItems = [...items, newItem];
    // put that into state
    setItems(nextItems);
  }
  
  return (
    <div className="wrapper">
      <div className="list-wrapper">
        <ol className="shopping-list">
          {items.map((item) => (
            <li key={item.id}>{item.label}</li>
          ))}
        </ol>
      </div>
      <AddNewItemForm handleAddItem={handleAddItem} />
    </div>
  );
}

export default App;
```

- **AC 2.** Submitting the form should add a new item to the list, and show it in the UI.

- On the `<form>` element, add an `onSubmit` handler to mange the form and button
- Add `event.preventDefault()` to stop the default browser behavior of reloading
- Call the `handleAddItem` function that was passed in via props from the parent component.
- Create a bit of state, to mange mange the new items. `const [label, setLabel] = React.useState('');`
- And pass the state `label` variable to the `handleAddItem()` function.

```JAVASCRIPT
  return (
    <div className="new-list-item-form">
      <form onSubmit={(event) => {
          // prevent the form from reloading
          event.preventDefault();
          // add a new item to the list, using new state management
          handleAddItem(label);
          // clear out the form, call the state function with empty string
          setLabel('');
        }
      }
      >
      // other stuff...
      </form>
    </div>
  );
}
```

- Now make the input controlled by the new state.
- Add `value={label}` to bind this to state
- Add a `onChange` handler to the input, to capture the content as use enters in the field.

```JAVASCRIPT
    <input
      id="new-list-form-input"
      type="text"
      value={label}
      onChange={(event) => {
        setLabel(event.target.value);
      }}
    />
```

- After you hit enter, you want to reset the form. `onSubmit` Call the set state function with an empty string. `seLabel('')`

- Good rule for managing state, you want to keep state as low as it can be, but no lower.

## Component Instances

- An important concept in React, and rarely discussed. Whenever we render a component in Rect, **we create a component instance**.
- A lot of outdated info on this topic. React has changed a ton since then.

- Video Summary:
- Where does state live? Look at a minimal React app:

```JAVASCRIPT
    import React from 'react';
    import { createRoot } from 'react-dom/client';

    function RandomNumber() {
      const [num, setNum] = React.useState(0);

      return (
        <button onClick={() => setNum(Math.random())}>
          Current number: {num}
        </button>
      );
    }

    const root = createRoot(document.querySelector('#root'));
    root.render(
      <>
        <RandomNumber />
      </>
    );
```

- In this example; we have a react app with a little bit of state. When you click the button, we set some state in the `num`. But where does this value come from? Where does it live?
- In this example; when we call `root.render` and call the component `<RandomNumber />` **we mount this component**.

- Mounting a Component consist of two parts:
  - 1. The process of figuring out what the JSX is. Everything in the component and processing it, handlers, DOM elements and any logic. Take this description and hand it over to React, and React produces that component.
  - 2. The second part, is we create a component instance, the very first time we render a component, it creates an object. And that object knows all the information about this particular instance of the component. Each one, gets it's own instance and manged by React.

- So we mount the component, which creates this instance, and over time as we are changing the value, that value is stored on this instance.

- In another example, if you remove an instance from the DOM, conditionally render a component based on some handler. React will destroy that instance and any state that is held within that component.
- If state is stored in an instance of a component, and that component is removed by React, than you loose anything stored in state.
- The example below, state is stored in the footer. When you toggle the button to display the footer. When you toggle the footer off, React removes that instance, destroys it from the DOM, and any state stored within the footer.

```JAVASCRIPT
import React from 'react';

function App() {
  const [
    includeFooter,
    setIncludeFooter,
  ] = React.useState(false);

  return (
    <>
      <h1>Some Application</h1>
      <form className="footer-toggle-wrapper">
        <input
          type="checkbox"
          id="footer-toggle"
          checked={includeFooter}
          onChange={(event) => {
            setIncludeFooter(event.target.checked);
          }}
        />
        <label htmlFor="footer-toggle">
          Toggle Footer
        </label>
      </form>
      {includeFooter && <Footer />}
  
    </>
  );
}

function Footer() {
  const [
    backgroundColor,
    setBackgroundColor,
  ] = React.useState('#232538');

  return (
    <footer style={{ backgroundColor }}>
      <form>
        <label htmlFor="bg-picker">
          Tweak background:
        </label>
        <input
          type="color"
          id="bg-picker"
          value={backgroundColor}
          onChange={(event) => {
            setBackgroundColor(event.target.value);
          }}
        />
      </form>
      <p>
        © Some Application Inc., 1998-present. All
        Rights Reserved.
      </p>
    </footer>
  );
}

export default App;
```

- What happens if you add multiple footers?

```JAVASCRIPT
{includeFooter && <Footer />}
{includeFooter && <Footer />}
{includeFooter && <Footer />}
```

- Each footer is a separate instance. And each have their own state variable.
- Each instance is independent.
- The moment we stop rendering that instance, that element gets deleted by React.
- 🤔 Every time you render a component, you create an instance.

- 📣 CORE CONCEPT: State is tied to a particular component instance, so if you unmount the component, and then re-mount it, that state is gone forever.
- 📣 Or if you have the same component rendered multiple times, they all have their own version of state, because they are a separate instance.

## State Management Tools

- One of the most common questions asked: "Should we use something like Redux to manage global state for use? Or is React capable enough on it's own?

- And the answer is, it depends.

### Some background info

- In the early days, React would only be one piece of your front-end stack. The best practice was to combine React with something like Flux, Redux, or MobX.

- The idea was to use React for *local state*, and tools like Redux for *global state*.
- "Local state" is state tht is only needed in one particular part of the application. Like the value of a controlled form, input or button.
- "Global State" is for broader things, like data about the currently logged in user, or maybe the currently selected color theme. A single piece of global state might be used in a dozen different corners of the app.

- From 2014 to 2016 a number of packages were competing to be the "global state" partner for React. The outcome of that was [Redux](https://redux.js.org).

### Intro to Redux

- Summary, In Redux, the global state is represented as a single object that floats outside our React application. This state can't be directly edited; instead, Redux listens fro "actions", events that signify that something happening in our app.

- All the action are logged by Redux. For example, a log for a typical e-commerce app.
  - User logs in
  - User submits search form
  - Search results received from server
  - User clicks on "Page 2" of search results
  - Search results received from server
  - User adds "Coffee  Machine" item to cart.

- Each of these actions fires through Redux, and we can write some code that controls how these actions should affect the state.

- Like in React, state updates in Redux are immutable.

### Application Types

How to choose when to use Redux vs. native React. Most web apps fall into three categories.

1. Not much state
2. Mostly client state
3. Mostly server state

- Not Much State - This is mostly websites, static sites, news websites, and blogs fall into this category. They can still have a lot of local state, but minimal with global state.

- Mostly Client State - This is, things that used to be a desktop application. Photo shop, video editors, word processors, all fit in this category. Lots of complex global state. We are just doing a lot of state manipulation in the browser. These are client heavy apps.

- Mostly server State - Web apps that work primarily with server state. A clear example is an analytics dashboard: A bunch of complex data in the database, and our app fetches that data and presents it to the user. Apps that are interfaces that let users read and manipulate data that lives in a database somewhere. Most CRUD (Create, Read, Update, Delete) fit into this category, most SaaS apps, social networks, search engines, e-commerce sites.

### The right tool for the job

The challenges are different for each category.

- For "Not much state" category, using just React exclusively for all state works great, not much to worry about with global state.

- Category 2, mostly client state, Redux is good choice. Redux helps to keep the code simple as it becomes more complex. And has a lot of built in performance optimizations.

- Category 3, mostly server state, most all network related.
  - Fetching the right date at the right time.
  - Making sure the data doesn't grow stale by revalidating it (re-fetching) and customizing it.
  - Caching the data sot hat multiple components don't repeat requests unnecessarily.
  - Optimistic UI updates, so that we predict how network requests will resolve.
  - Pagination, requesting small slices of the data and letting the user pull new data as needed.

- And, Redux doesn't really address any of those things, Redux is a state management tool, does not help with network stuff.

#### Network tools

- Tools to help us mange network related challenges.
  - [Apollo](https://www.apollographql.com)
  - [react-query](https://tanstack.com/query/v3/)
  - [SWR](https://swr.vercel.app)

- If you are building a video game, or audio editor, Redux is great. Most likely you are building in either Category 1 (Not much state), or Category 2 (Mostly server state)

#### Mental Model to figure out which tool

1. If it's a static website, use React alone, with the help of context
2. If client-heavy app, use Redux for global state
3. If it's a server heavy app, use React + something to help with the network stuff. For this course platform, Josh uses's Vercel's SWR.

**However**, React has evolved over the past few years, you can build large, sophisticated apps without any other state-management libraries.

🤔 If you are new to react, spend a year or two getting comfortable building apps. Then expand to other tools.

### Redux Toolkit and RTK-Query

- [Redux Toolkit](https://redux-toolkit.js.org), an opinionated toolset that aims to sand down some of the rough edges of working with Redux.
-Josh prefers the "Classic Redux" without the toolkit.
- If already using Redux Toolkit, makes sense to use RTK Query.

### Bonus: Input Cheatsheet

- [Going to link to this](https://courses.joshwcomeau.com/joy-of-react/02-state/11-bonus-cheatsheet), to much to write up.
- Text Inputs
- Textareas
- Radio buttons
- Checkboxes
- Select
- Specialty Inputs, range, data, color

## Project: Word Game

- In this project, we will create a clone of the popular game, Wordle.
- In Wordle, you get a new word each day, and it's your challenge to guess what the is, in six guesses or less.
- You use the colors of background, to figure out what should go where.
- Touches on a lot of the core concepts we learned in previous lessons.

### Strategies and Mindset

- This project is broken down into 5 discrete exercises. We will build the Wordle clone step by step, working up to the finished game. Each one with it's own solution, and video walkthrough.

- The flow of this project, for this project:

1. Follow the instructions in the project's README.md, attempt to solve the first exercise.
2. Once you have solved, or attempted to solve, watch the solution video for the first exercise.
3. Repeat steps 1 and 2 for all 5 exercises.

#### Growth mindset

- Struggling and failing is much more productive than effortlessly breezing through an easier challenge. In this exercise, you will feel frustrated to solve, try and reframe it as a successful learning experience. Failure is inherently frustrating.

## Local Development

- The project source is here, <https://github.com/joy-of-react/word-clone>

- This is a short lesson on getting the project running.

### Node, NPM and command line

- If new to modern front end development, you can check out the [tools of the trade](https://courses.joshwcomeau.com/joy-of-react/11-tools-of-the-trade/00-introduction) lessons.
- Covers the terminal, code editor, Node.js and npm.

- Modern terminal tools
  - [Hyper](https://hyper.is)
  - VS Code terminal editor, View > Terminal in the VS Code app
  - [iTerm2](https://iterm2.com)
  - [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=en-us&gl=us)

- Shell language, when you use the terminal and press enter. That command will be interpreted by the shell language. The environment running within the terminal language.

- **Bash**, is the most popular shell language. Most of the time, online instructions will be in Bash. It is the default shell language used by most Linux distributions.

- **Zsh**, most modern MacOS versions ship with Zsh instead of Bash. And are very similar and share almost all the same commands. Can be used interchangeably.

- If you are using either Linux or MaxOS, you are good to go. Your computer already has "industry standard" shell language. If on Windows, you have some setup work todo.

- ℹ️ Skip the $, in most instructions you will see a command like this. `$ npm install some-package`
- The dollar sign isn't meant to be included. You are meant to type everything after the dollar sign.
- In the Bash shell language, the `$` is the prompt character, shown to indicate that the terminal is ready to receive a command.

- ℹ️ Exiting Vi / Vim, these are a PITA to exit.
- To quit without saving, use these steps:
  - Press `Escape`
  - Press `:` this should add the prompt at the bottom of the terminal.
  - Type `q!`
- If you'd like to save before quitting, you'd type `wq` (Write Quit) instead of `q!`.

### Tip N' Tricks on Terminal

- **Toggling command**, The `-` character will toggle you in between working directories, `cd -` in the terminal, kinda like the previous button on your tv remote, you can jump back and forth.

- **Clearing the terminal**, you can type `clear` and erase all commands. You can also use the shortcut, `ctrl` + `L`.

- **Switching to the GUI file explorer**, in MacOS, you can just enter `open .` to open the finder window for that directory.

- **Chaining commands**, you can chain two commands together, using the `&&` operator. The first command runs, then the second automatically. For example; `npm install && npm run start`

- **Terminal tiling and tabs**, when running a dev server, it is a long running process and is not able to accept additional commands. Modern terminal apps, you can run many terminal sessions in the same application. MacOS, the shortcut is `Shift` + `Command` + `d`.

- To run a new tab, just do `Command` + `t`

- ℹ️ Tip, you can use a window position manager, like [Rectangle](https://rectangleapp.com) or Moom

- ℹ️ VS Code Tip, to open VS Code from the MacOS terminal, `code .`, first you need to set it up in VS Code.
  - In VS Code, open the command palette with `Command` + `Shift` + `P`
  - Type `Shell` in the command palette
  - Select `Shell Command: Install code in PATH`

- ℹ️ VS Code Tip, to view the markdown, run the command palette, `Command` + `Shift` + `P`, and type, `markdownopen` to view a rendered view of the markdown. or `Markdown: Open Preview`

### Running the local dev server

- Download the app from git, <https://github.com/joy-of-react/word-clone>
- cd into the project
- Run `npm install`
- And run the app, `npm run start`
- View the `README.md`

## Getting Started

- Fork the repo, on your own github, so you can submit it and review the solution videos.
- The README.md is the file you want to follow, to go through each exercise.

- My repo, forked copy: <https://github.com/clewisdavis/joy-of-react-word-clone>

- In Wordle, users have 6 attempts to guess a 5-letter word. You are helped along the way by ruling out letters that are not in the word, and being told whether the correct letters are in teh correct location or not.

### Exercise 1: GuessInput

- First thing, we need a way to submit guesses.
- We need to render a little form that holds a text input:
- Create a new component for this UI, and render it inside the `Game` component.

- **Acceptance Criteria:**
- Create a new component, don't forget to use an NPM script to generate the scaffolding for you.
- This component should render a `<form>` tag, including a label and a text input.
- The text input should be controlled by React state.
- When the form is submitted:
  - The entered value should be logged to the console for now.
  - The input should be reset to an empty string.
- The user's input should be converted to ALL UPPERCASE. No lower case letters allowed.
- The input should have a minimum and maximum length of 5.
- Note: The `minLength` validator is a bit funky; you may wish to use the `pattern` attribute instead. This is discussed in mor detail on the Solution page.

- Solution, GuessInput Component

```JAVASCRIPT
import React from "react";

function GuessInput() {

  const [guess, setGuess] = React.useState('');

  function handleSubmit(event) {
    // Prevent the default browser behavior 
    event.preventDefault();
    // log to console, the {} renders in console as a object
    console.log({ guess });
    // reset the input
    setGuess('');
  }

  return (
    <>
     <form 
       className="guess-input-wrapper"
       onSubmit={handleSubmit}
      >
        <label htmlFor="guess-input">Enter guess:</label>
        <input 
          required
          minLength={5}
          maxLength={5}
          pattern="[a-zA-Z]{5}"
          title="5 letter word"
          id="guess-input"
          type="text"
          value={guess}
          onChange={ (event) => {
            const nextGuess = event.target.value.toUpperCase();
            setGuess(nextGuess);
          }}
          />
    </form>
    </>
  );
}

export default GuessInput;
```

## Exercise 2: Keeping track of the list

- The goal of this exercise is to render each of the user's guesses:

- Sample DOM structure

```JAVASCRIPT
<div class="guess-results">
  <p class="guess">FIRST</p>
  <p class="guess">GUESS</p>
</div>
```

- **Acceptance Criteria:**

- 1. A new component should be created, to render previous guesses.
- 2. When the user submits their guess, that value should be rendered within this new component.
- 3. There should be no key warnings in the console!

- Video Solution Notes:
- This exercise is tricky, seems simple, but how do you manage the state that we already have.
- The standard solution, to access data from one component to another, is to lift the state up.
- But, that does not work in this case. In this game, we only want to show the guess once it is submitted.
- When you go down the path of just lifting the state up. You see the guess as you type it, in the input field.
- What we want, the moment you type hello, and press enter, you see the guess. Transfer it, leaves the input and shows up in the list.

- Instead, you want to track, juggle the data between two bits of state.
- Create some new state, on the parent component `<Game/>`, that starts as an empty array.

```JAVASCRIPT
  const [guesses, setGuesses] = React.useState([]);
```

- Then write a new handler on the input `<GuessInput handleSubmitGuess={handleSubmitGuess}` and write the new function.
- This function will take the guess as an argument and update the guess to include in the array.

```JAVASCRIPT
  function handleSubmitGuess(guess) {
    // TODO
  }
```

- In the `<GuessInput/>` component, pass the handler in as an argument. So it can receive it.

```JAVASCRIPT
  // GuessInput.js
  function GuessInput({ handleSubmitGuess }) {
    // Component here
  }
```

- 🚀 In the `onSubmit` handler, for the input, pass that value up to the parent component, `Game.js`.

```JAVASCRIPT
  // GuessInput.js
  function handleSubmit(event) {
    // pass the value of guess up
    handleSubmitGuess(guess);
  }
```

- In the parent component, `<Game/>`, you can console.log out the guess to make sure the data is being passed from one component to another.

```JAVASCRIPT
  // Game.js
  function handleSubmitGuess(guess) {
    // Check to see if the data is being passed.
    console.log(guess);
  }
```

- In the handler, you need to add the new guess that was submitted, into the state, array. `guesses`
- Call the `setGuesses()` method to push the new guess into the array.
- Remember the rule, you are not allowed to modify existing arrays or objects.
- So we have to spread the current guess, and add the new guess

```JAVASCRIPT
  // Game.js
  function handleSubmitGuess(guess) {
    // Set state method, spread syntax, creating an array that includes all the current guesses. And add the new guess. 
    // Creates a new array, similar to array.push(), instead of modifying an existing
    setGuesses([...guesses, guess]);
  }
```

- JS Primer on the [spread syntax](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/12-rest-spread)

- Pass the array of guesses to the `<GuessResults guesses={guesses} />` component, so you can display it.
- Then we need to map over the guesses.

- Is this a bad pattern, to copy between state? Not in this case, only when you have the exact same information, that is spread across multiple state.
- But here, we have state in the input, then when you hit enter, it is being transferred to the parent level array, the list.
- We are transferring the source of truth.

- Now to render the guesses using `map()`, map over the state

```JAVASCRIPT
  function GuessResults({ guesses }) {
    return (
      <div className="guess-results">
        // map over state
        {guesses.map(guess => (
          <p className="guess">{guess}</p>
        ))}
      </div>
    )
  }
```

- Now you need to apply a unique key prop to each guess.
- You can use `index` for key, but be aware, of the re-order issue. Does you application allow for re-order.

```JAVASCRIPT
  function GuessResults({ guesses }) {
    return (
      <div className="guess-results">
        // add the key, using index
        {guesses.map((guess, index) => (
          <p key={index} className="guess">{guess}</p>
        ))}
      </div>
    )
  }
```

- What if you cannot use the `index` for the key, if the order changes or swap?
- You would have to create a unique identifier with each guess. And that happens in your `handleSubmitGuess()` handler.

```JAVASCRIPT
  // Game.js
  function handleSubmitGuess(tentativeGuess) {

    // Create an object to hold the value and the unique id
    const nextGuess = {
      value: tentativeGuess,
      id: Math.random(), // set a unique id, 15 digits
    }
    // set that in the array for state, so now every item is going to be an object
    setGuesses([...guesses, nextGuess]);
  }
```

- Now in your `<GuessResults />` component.
- You can deconstruct that `{value, id}` and then use it on the component.

```JAVASCRIPT
  function GuessResults({ guesses }) {
    return (
      <div className="guess-results">
        // add the key, using index
        {guesses.map(({value, id}, index) => (
          <p key={id} className="guess">{value}</p>
        ))}
      </div>
    )
  }
```

## Exercise 3: Guess Slots

- Hint: You will need to generate a bunch of squares for each guess. This can be done using the provided [range utility](https://courses.joshwcomeau.com/joy-of-react/01-fundamentals/08-range-util).

- In this exercise, we will update our code to display 6 rows of guesses, no matter how many guesses the user has submitted, and each row will consist of 5 cells.
- As the user submits guesses, their guess will populate the cells:

- Starter markup for the slots

```HTML
<div class="guess-results">
  <p class="guess">
    <span class="cell">F</span>
    <span class="cell">I</span>
    <span class="cell">R</span>
    <span class="cell">S</span>
    <span class="cell">T</span>
  </p>
  <p class="guess">
    <span class="cell">G</span>
    <span class="cell">U</span>
    <span class="cell">E</span>
    <span class="cell">S</span>
    <span class="cell">S</span>
  </p>
  <p class="guess">
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
  </p>
  <p class="guess">
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
  </p>
  <p class="guess">
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
  </p>
</div>
```

- Things to know to help with this exercise:

1. You can use the `range` utility to create array sof a specified length to map over. It's provided in `/src/utils.js`. Check out range utility lesson for more info.
2. Inside `/src/constants.js`, you find a constant, `NUM_OF_GUESSES_ALLOWED`. You should import and use this constant when generating the set of guesses. This would make it easy for us to change the difficulty of the game later on, bu increasing/decreasing the # of guesses.

- Acceptance Criteria:
  - Create a new `Guess` component. 6 instances should be rendered at all times, no matter how many guesses have been submitted.
  - The `Guess` component should render 5 spans, each with the class of `cell`.
  - Each cell should contain a letter, if the `Guess` instance has been given a value. If not, the cell should be blank.
  - Use the `NUM_OF_GUESSES_ALLOWED` constant, when needed.
  - No `key` warnings in the console.

- Create a new component, but how do you split up the guesses and put them in each box?

```JAVASCRIPT
    function Guess({ value }) {
      return (
        <>
          <p className="guess">
            {
              // split up the guesses and map through
              value.split('').map((letter, index)) => (
                <span key={index} className="cell">
                  {letter}
                </span>
              )
            }
          </p>
        </>
      )
    }
```

## Exercise 4: Game logic

- In this exercise, we will add some CSS classes to color the background of each cell, based on the results and the correct answer:

- Inside `/src/game-helpers.js` you will find a helper function, `checkGuess`. As parameters, it takes a single guess, as well as the correct answer. It returns an array that contains the status for each letter.

- For example:

```JAVASCRIPT
checkGuess('WHALE', 'LEARN');
/*
  Returns:

  [
    { letter: 'W', status: 'incorrect' },
    { letter: 'H', status: 'incorrect' },
    { letter: 'A', status: 'correct' },
    { letter: 'L', status: 'misplaced' },
    { letter: 'E', status: 'misplaced' },
  ]
*/
```

- There are 3 possible statuses:
  - correct - this slot is perfect.
  - misplaced - this letter does exist in the word, but in a different slot.
  - incorrect - this letter is not found in the word at all.

- In the example above, `W` and `H` are not found in the word `LEARN`, and so they are marked as "incorrect". `A` is correct, since it is in the 3rd slot in each word. The other two letters `L` and `E` are meant to be in other slots.

- These statuses correspond with CSS classes. the `correct` status has a `correct` class name, which will apply the green background when applied to a cell. Same thing for `misplaced` and `incorrect`.

- The task is to use this function to validate the user's guesses, and apply the correct CSS classes. The final output for a given guess should look like this:

```HTML
<p class="guess">
  <span class="cell incorrect">W</span>
  <span class="cell incorrect">H</span>
  <span class="cell correct">A</span>
  <span class="cell misplaced">L</span>
  <span class="cell misplaced">E</span>
</p>
```

- Acceptance Criteria:
  - Import the `checkGuess` function from `/src/game-helpers.js` and use it to validate each of the user's guesses.
  - When rendering the letter in the `Guess` component, apply the letter's `status` to the `cell` element.
  - Empty guess slots should have the same markup as before: `<span class="cell"></span>`

## Exercise 5: Winning and loosing

- The last thing to do, is to end the game.
- If the user wins the game, a happy banner should be shown:
- If the user loses teh game, by contrast, a sad banner should be shown:

- The user wins teh game when their guessed word is identical to the `answer`. They lose the game if they submit six guesses without winning.

- When the game is over, one of these banners should be shown, and the text input should be disabled sot hat no new guesses can be typed or submitted.

**- Acceptance Criteria:**

- If the user wins the game, a happy banner should be shown.
- If the user loses the game, a sad banner should be shown.
- When teh game is over, the text input should be disabled.
- It is up to your to decide hwo to structure the banner. Feel free to create a new component if you think it's warranted.

## Module 3: React Hooks

### Another tool in the Toolbox

- A way to share business logic throughout our app, without having to stack a bunch of components.
- The tool to solve this, is Hooks.
- Allow you to reuse logic throughout the application.
- Use it to manage a event listener, or data request, can now be packed up and re-used.
- Using Hooks and components together, makes building in React much better.

### The useId Hook

- Reuse, is the main benefit of using react.
- Try to write, any component re-usable. But is difficult for some parts of the web.
- React has a new tool, to give a unique identifier to an instance of a component.

```JAVASCRIPT
const id = React.useId();
```

- Gives a unique ID, to each instance of a given component. For example; if you have a form, and you want to give a unique id for each text input, label.

- If you console log the `id`, you get a unique id that corresponds with each instance of the component.
- React knows, how many instances of a given component exist, and it stores that info on the instance.
- The useId Hook, let's us hook into that internal data and access that id.

- Now you can use that unique id, on a form, and create a unique string.

```JAVASCRIPT
import React from 'react';

function LoginForm() {
  const [username, setUsername] = React.useState('');
  const [password, setPassword] = React.useState('');

  // Pluck this instance's unique ID from React
  const id = React.useId();

  // Create element IDs using this unique ID
  const usernameId = `${id}-username`;
  const passwordId = `${id}-password`;
  
  return (
    <form className="login-form">
      <div>
        {/* Apply these IDs to the label and input */}
        <label htmlFor={usernameId}>
          Username:
        </label>
        <input
          type="text"
          id={usernameId}
          value={username}
          onChange={event => {
            setUsername(event.target.value);
          }}
        />
      </div>
      <div>
        <label htmlFor={passwordId}>
          Password:
        </label>
        <input
          type="password"
          id={passwordId}
          value={password}
          onChange={event => {
            setPassword(event.target.value);
          }}
        />
      </div>
      <button>
        Submit
      </button>
    </form>
  );
}

export default LoginForm;
```

- How about using an alternative to create a unique id, like `Math.random()`
- But when you enter information into the field, you trigger a state change, and a new number will be created with the `Math.random()`. Notice the `React.useId()` is stable across renders.
- Because we are hooking into an instance, and the instance doesn't change between renders. But the `Math.random()` does change with each re-render of the component.
- Whenever we re-render the component, we call the function again, so `Math.random()` will generate a new number.
- Special thing with Hooks, we are able to pass things along between renders.

### Practice - Toggle Component

- A `Toggle` component, finish it up by adding a unique ID to the button, and connecting it ot the label. You should be able to trigger the toggle by clicking the 'Dark Mode' text.

- Add the `useId()` method hook. `const id = React.useId();`
- On the switch element label, add the `htmlFor` attribute, `htmlFor={id}`
- Then add the associated `id={id}` to the `button`

```JAVASCRIPT
function Toggle({
  label,
  checked,
  handleToggle,
  backdropColor = 'white',
  size = 16,
}) {
  const id = React.useId();

  const padding = size * 0.1;
  const width = size * 2 + padding * 2;

  const wrapperStyle = {
    width,
    padding,
    '--radius': size * 0.25 + 'px',
    '--backdrop-color': backdropColor,
  };

  const ballStyle = {
    width: size,
    height: size,
    transform: checked ? `translateX(100%)` : `translateX(0%)`,
  };

  return (
    <div className={styles.wrapper}>
      <label htmlFor={id}>
        {label}
      </label>
      <button
        id={id}
        className={styles.toggle}
        type="button"
        aria-pressed={checked}
        style={wrapperStyle}
        onClick={() => {
          handleToggle(!checked)
        }}
      >
        <span className={styles.ball} style={ballStyle} />
      </button>
    </div>
  );
};
```

### Rules of Hooks

- Hooks are special functions that allow us to "hook" into Rect internals.
- `useState` allows us to hook into a component instance's state, for example, while `useId` allows us to create and store a unique identifier on the component instance.
- What happens if we try to call these functions outside of a react context?
  - You get a warning, stating Hooks can only be called inside a body of a function component. This could happen for one of the following reasons.
  - 1. You might have mismatching versions of React and teh renderer, such as React DOM
  - 2. You might be breaking the Rules of Hooks
  - You might have more than one copy of React in the same app.

- First understand that hooks are plain old JS functions.
- When we call these functions, they hook into React internals.
- React expects hook functions to be used in very specific ways, and if we violate those expectations. Base things can happen.

- 📣 There are two Rules of Hooks that we should learn.
- 1. Hooks have to be called within the scope of a React app. We can't call them outside of our React components.
- 2. We have to call our hooks at the top level of the component. **This rule trips most people up.**

- Cannot call a hook, if it's in a condition statement, or anything that makes it not at the top level of the component. Order matters.
- A nice way to look at it, if the hook is indented, will not work.

```JAVASCRIPT
// will error
function TextInput({ id, label, type }) {
  let appliedId = id;
  if (typeof appliedId === 'undefined') {
    appliedId = ReactuseId();
  }

  return (
    // component markup
  )
}
```

- What if you wanted to use a condition? How would you write that?

```JAVASCRIPT
import React from 'react';

function TextInput({ id, label, type }) {
  // Here's the original code, violating the rule:
  //
  //   let appliedId = id;
  //   if (typeof appliedId === 'undefined') {
  //     appliedId = React.useId();
  //   }
  //
  // ...and here's the fixed code:
  const generatedId = React.useId();
  const appliedId = id || generatedId;

  return (
    <div className="text-input">
      <label htmlFor={appliedId}>
        {label}
      </label>
      <input
        id={appliedId}
        type={type}
      />
    </div>
  );
}

export default TextInput;
```

#### Hooks, exercises

- Fix the violation
- We have a search UI with a conditionally rendered text input. Clicking the icon slides a text input into place.
- The code is a violation of Rules of Hooks! Your mission is to update the code to comply with the rules.

- ACs
  - No lint warnings should be shown.
  - Clicking the search button should reveal a search input.

- Two possible solutions, first is to make sure the hook is at the correct level on the component.

```JAVASCRIPT
// Change this code:
let searchId;
if (showSearchField) {
  searchId = React.useId();
}

// ...to this code:
const searchId = React.useId();
```

- Or you can create a new create a new component and conditionally display the search input.

```JAVASCRIPT
import React from 'react';
import { X, Search } from 'react-feather';

import VisuallyHidden from './VisuallyHidden';

function App() {
  const [
    showSearchField,
    setShowSearchField,
  ] = React.useState(false);

  function handleToggleSearch(event) {
    event.preventDefault();
    setShowSearchField(!showSearchField);
  }

  return (
    <>
      <form>
        {showSearchField && <SearchField />}
        <button
          className="search-toggle-button"
          onClick={handleToggleSearch}
        >
          {showSearchField ? <X /> : <Search />}
          <VisuallyHidden>
            Toggle search field
          </VisuallyHidden>
        </button>
      </form>
    </>
  );
}

// create a new component, and use logical && to show it above
function SearchField() {
  const searchId = React.useId();

  return (
    <div className="search-field-wrapper">
      <label htmlFor={searchId}>
        <VisuallyHidden>Search</VisuallyHidden>
      </label>
      <input
        id={searchId}
        className="search-field"
      />
    </div>
  );
}

export default App;
```

#### Exercise, Fix another violation

Fix another violation with a different scenario

- In this exercise, our LoginForm component can either render:
- A form including email/password fields, if the user isn't logged in.
- A paragraph, if the user is logged in

You can toggle between these states by submitting the form.

- ACs
- Update the code below so that it doesn't violate the Rules of Hooks.

- Solution 1. Moving the early return, grouping the hooks together before you return any conditions.

```JAVASCRIPT
function LoginForm({ isLoggedIn, handleLogin }) {
  const id = React.useId();
  const [email, setEmail] = React.useState('');
  const [password, setPassword] = React.useState('');

  if (isLoggedIn) {
    return (
      <p>You're already logged in!</p>
    );
  }

  return (
    /* Same stuff, omitted for brevity */
  );
}
```

- Solution 2. Removing the early return entirely. Move the `isLoggedIn` to the parent component, `App`
- Use a ternary condition to display the logged in status.

```JAVASCRIPT
function App() {
  const [isLoggedIn, setIsLoggedIn] = React.useState(false);

  function handleLogin(event) {
    // NOTE: In a real application, we'd perform a
    // network request here, to validate the login.
    // We'll see how to do this later in this module.
    event.preventDefault();
    setIsLoggedIn(true);
  }

  return (
    <>
      {isLoggedIn ? (
        <>
          <p>You're already logged in!</p>
          <button
            onClick={(event) => {
              setIsLoggedIn(false);
            }}
          >
            Log Out
          </button>
        </>
      ) : (
        <LoginForm handleLogin={handleLogin} />
      )}
    </>
  );
}
```

### Immutability Revisited

One of the mis-understood things to grasp with React, is immutability, and what it means for state to be immutable.

- Remember, in React, you never want to mutate a piece of state.
- If you want to change a piece of state, you have to call the `setUser()` function. And provide it with a new object with the properties you want.
- When you do that, you are actually creating a new object, that lives in a different part of the computers memory.
- The confusing part, you are not editing the original object, you are creating a new one. And when you create a new one, you are not swapping out the instance, you are just creating a new one.

```JAVASCRIPT
// setUser, set state
user: {
  name: "Ivy"
}
```

- And another one

```JAVASCRIPT
// setUser, set state
user: {
  name: "Ava"
}
```

- The CORE idea, is when you setUser State, you have two different user variables in memory.
- 💡 So when we say, we are changing the state, it's more like we are painting a new picture with the new value. Even if the new value is the exact same.

- The moment you create this new object, it will be stored in memory. Imagine a ram stick, and it stores it in a little square on that ram stick.
- All that matters to React, is I am changing the object, and for it to use the new object in the next snapshot.

- Now, how does this work when you have multiple state variables.
- A state variable and an array, these will take up two differ slots in memory.

```JAVASCRIPT
user : {
  name: "Ivy"
}

items: [
  1
]
```

- Imagine you go through the process of calling setUser, with a handful of new objects.
- Every time I create a new object by calling `setUser`, I get a new object, that is put into a new memory slot in the computers memory.

- But not for the items array, each snapshot of the items array, points to the same spot in the computers memory.

```JAVASCRIPT
user : {
  name: "Ivy"
}

items: [
  1
]
```

- We are still generating a brand new items array in each snapshot, with `const [items, setItems] = React.useState();`.
- Each of the items is a different locally scoped variable, BUT the all point to the same underlying reference in memory.
- I only have one array of numbers and it's being threaded through every single snapshot.

- Have to understand the difference between two items that are referentially different, vs. the same item being threaded through. 🤔
- 🤔 The core idea, is how React handles mutability and how data is passed through snapshots.

- **What is a snapshot?**

- A snapshot is the result of performing a render. It's a combination of two things:

1. The specific values of any props/state at the time the render occurred
2. The React elements return form the component, describing the UI calculated in the render

- What is the difference between snapshots and instances?
- A component instance, is a JS object that is the source of truth for everything related to a particular instance of a component. It's created when teh component is mounted and it persists until the component is unmounted.

- A snapshot, is not a specific JS object, it refers to the data available at a moment in time.
- Could say, an instance holds the true value of a piece of sate, **but every time that state change**, we create a snapshot that captures the current value of that state variable.

### Refs

- How might we work with the `<canvas>` element in React?
- In this case, if you wanted to work with the `<canvas>`, you would have to get a context, a reference to that element.
- In JS, we do this by, using the `document.querySelector('canvas')` api, to get a reference to that element.
- But to do this in React, considered a bad practice, to reach around React, and get a reference to an element.
- This will also lead to bugs, when you have multiple instances of that component.

- To get the context, or reference of an instance, the conventional way is to use the `ref` attribute, hook.
- Its a little like the `key`, it signals to React we want to do something special.
- One way, is we can pass it a function, and React will provide us, with the underlying DOM node.

```JAVASCRIPT
function ArtGallery() {
  return (
    <main>
     <canvas
       ref={function(canvas) {
        console.log(canvas)
       }}
       width={200}
       height={200}
    />
    </main>
  )
}
```

- In the console,  you get the actual `<canvas>` element, the same thing you would get, if you called `document.querySelector('canvas')`
- This is one way to get to the underlying DOM node.

- The way `ref` works, it will be called whenever the component renders.

- A hook to use, in these situations, called `React.useRef()`
- The idea with `useRef()` is that it creates a box, and we can put whatever we want in the box, and React will thread this through every single render, so that I always have access to it.

- By default, `React.useRef()` creates an object, set to `{current: undefined}`
- Similar to the `React.useState()` hook, you can pass an initial value if you want.

```JAVASCRIPT
function ArtGallery() {
  const canvasRef = React.useRef();

  return (
    <main>
     <canvas
       ref={canvasRef}
       width={200}
       height={200}
    />
    </main>
  )
}
```

- You don't want to re-assign the variable, you want to mutate the `useRef` object.
- Hold up, aren't you not supposed to mutate object?
- That the difference, `React.useState()` is supposed to be immutable, BUT `React.useRef()` is intended to be mutated.

- Breakdown what doing:
- Creating a variable, `canvasRef` that starts as `{current: undefined}`
- Then you are capturing a reference, to a particular DOM node, by setting the `ref={canvasRef}` within the element, in this case, it's the `<canvas />` DOM node.
- This sets the `React.useRef()` hook to be set to the element, in this case `{ current: <canvas> }`

- Full Code Sample:

```JAVASCRIPT
import React from 'react';

function ArtGallery() {
  // 1. Create a “ref”, a box that holds a value.
  const canvasRef = React.useRef(); // { current: undefined }

  return (
    <main>
      <div className="canvas-wrapper">
        <canvas
          // 2. Capture a reference to the <canvas> tag,
          // and put it in the “canvasRef” box.
          //
          // { current: <canvas> }
          ref={canvasRef}
          width={200}
          height={200}
        />
      </div>

      <button
        onClick={() => {
          // 3. Pluck the <canvas> tag from the box,
          // and pass it onto our `draw` function.
          draw(canvasRef.current);
        }}
      >
        Draw!
      </button>
    </main>
  );
}

function draw(canvas) {
  const ctx = canvas.getContext('2d');

  ctx.clearRect(0, 0, 200, 200);

  ctx.beginPath();
  ctx.rect(30, 90, 140, 20);
  ctx.fillStyle = 'black';
  ctx.fill();
  ctx.closePath();

  ctx.beginPath();
  ctx.arc(100, 97, 75, 1 * Math.PI, 2 * Math.PI);
  ctx.fillStyle = 'tomato';
  ctx.fill();
  ctx.closePath();

  ctx.beginPath();
  ctx.arc(100, 103, 75, 0, 1 * Math.PI);
  ctx.fillStyle = 'white';
  ctx.fill();
  ctx.closePath();
  
  ctx.beginPath();
  ctx.arc(100, 100, 25, 0, 2 * Math.PI);
  ctx.fillStyle = 'black';
  ctx.fill();
  ctx.closePath();
  ctx.beginPath();
  ctx.arc(100, 100, 19, 0, 2 * Math.PI);
  ctx.fillStyle = 'white';
  ctx.fill();
  ctx.closePath();
}

export default ArtGallery;
```

### Exercises, Refs

- Video playback speed - we have a `<VideoPlayer> component that includes a playback speed control. But it doesn't work.
- For context, in vanilla JS, you can affect the playback speed of a `<video>` element with the following:

```JAVASCRIPT
const videoElement = document.querySelector('#some-video');
videoElement.playbackRate = 2; // Play at 2x speed
```

- AC's
- When the use changes the "Playback speed" control adn then plays the corresponding video, that video should play at the selected speed.
- You should use the `useRef` hook to capture a ref tot he `<video>` element.

- My attempt:

```JAVASCRIPT
import React from 'react';

function VideoPlayer({ src, caption }) {
  const playbackRateSelectId = React.useId();

  const videoRef = React.useRef(); // {current: undefined}
  console.log(videoRef);
  
  return (
    <div className="video-player">
      <figure>
        <video
          controls
          src={src}
          ref={videoRef}
        />
        <figcaption>
          {caption}
        </figcaption>
      </figure>
      
      <div className="actions">
        <label htmlFor={playbackRateSelectId}>
          Select playback speed:
        </label>
        <select
          id={playbackRateSelectId}
          defaultValue="1"
          onChange={(element) => {
            // grad the reference element
            const videoElement = videoRef.current;
            // grab the value of the select
            const videoRate = element.target.value;
            console.log(videoRate);
            // set the playback on the element and set it equal to the value of the select
            videoElement.playbackRate = videoRate;
          }}
        >
          <option value="0.5">0.5</option>
          <option value="1">1</option>
          <option value="1.25">1.25</option>
          <option value="1.5">1.5</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>
      </div>
    </div>
  );
}

export default VideoPlayer;
```

- Solution video:
- Same solution, but you could simplify the `onChange` within the select.

```JAVASCRIPT
onChange={(event) => {
  videoRef.current.playbackRate = event.target.value;
}}
```

- NOTE: Don't forget the `.current` after.

### Exercise, Media Player

- Let's build a media player!

- The UI is ready and we have loaded an audio file, using `<audio>` tag. Our job now is to capture a reference to that element, and to trigger it when the user clicks teh play/pause button.

- For context, here how we solve this problem in vanilla JS:

```JAVASCRIPT
const audioElement = document.querySelector('#some-audio-element');

// Start playing the song
audioElement.play();

// Stop paying the song
audioElement.pause();
```

- AC's
- Clicking the "Play" button should start playing the song.
- Clicking the button again should pause the song.
- By default, we should render a `<Play>` icon inside the button, but it should flip to a `<Pause>` icon while the song is playing.

- Hint: to keep track whether the song is currently playing or not, you should use React state variable.
- In general, if you want some part of the UI to update, you need to use `useState()` hook.

- My attempt:

```JAVASCRIPT
import React from 'react';
import { Play, Pause } from 'react-feather';

import VisuallyHidden from './VisuallyHidden';

function MediaPlayer({ src }) {

  // use state to manage flipping the pause / play button
  const [isPlaying, setIsPlaying] = React.useState(false);
  
  const audioRef = React.useRef(); // {current: undefined}
  
  return (
    <div className="wrapper">
      <div className="media-player">
        <img
          alt=""
          src="https://sandpack-bundler.vercel.app/img/take-it-easy.png"
        />
        <div className="summary">
          <h2>Take It Easy</h2>
          <p>Bvrnout ft. Mia Vaile</p>
        </div>
        <button
          onClick={() => {
            // use the state variable, set to false by default
            if (isPlaying) {
              audioRef.current.pause();
            } else {
              audioRef.current.play();
            }

            // flip the setState variable from true false or false true with the Logical NOT expression !
            setIsPlaying(!isPlaying);
                
          }}
        >
        
          { // use the isPlaying variable, to show which icon is shown
          isPlaying ? <Pause /> : <Play/>
          }
 
          <VisuallyHidden>
            Toggle playing
          </VisuallyHidden>
        </button>
        
        <audio 
          src={src}
          ref={audioRef}
        />
      </div>
    </div>
  );
}

export default MediaPlayer;
```

- Additional solution: When the song finishes, we need to slip the icon back to the initial state with `onEnded` added to the `<audio>` element.

```JAVASCRIPT
<audio
  ref={audioRef}
  src={src}
  onEnded={() => {
    setIsPlaying(false);
  }}
/>
```

## Side Effects

- As we build apps, we often need to synchronize with external systems. Things like.
  - Making network request
  - Managing timeouts / intervals
  - Reading / writing from localStorage
  - Listening for global events

- React calls all of these things 'side effects'.

### About "Best Practices"

- So, these side effects are hard, one of the hardest things about a modern React app.
- Should never feel shame for writing code that is not perfect and follow the best practices.
- Where ever you go, everyone is learning it as you go, and it's more about the journey and growth of learning.
- You will never be in a place in your career where you are writing/designing perfect code or designs. You are always learning. And if you look back at your code or designs from a couple years ago, and feel embarrassed, that is a good sign. That you are growing and not feeling stagnated.

### The useEffect hook

- So far, we have discussed the **CORE React Loop**
  - Provide React the JSX, a description of the UI you want.
  - Then React takes those descriptions and produces the real world DOM the user can see and use.
  - When the use does something, state change for example, React re-runs all the code and give React a new description of the DOM.

- But in web apps, we need to do things outside of this. For example, updating the title of the tab.
- This falls outside of Reacts normal space. Now we want to change something that exist outside of that container.
- React has a name for this, Side Effects, this is when you want to do something that is typically outside of Reacts typical job responsibilities. But still needs to be synchronized with the state we are holding in React.

- We want to synchronize this external element, with React state.
- To do this, we use `React.useEffect()`
- How it works, we pass it a function. And React will call this function for us at appropriate times.

- Example, update the title tag, to use a count state variable.

```JAVASCRIPT
// Counter.js

function Counter({ name, initialVal = 0 }) {

  // state management
  const [count, setCount] = React.useState(initialVal);

  React.useEffect(() => {
    document.title = `(${count}) - Counter 2.0`;
  })
  return (
    // DOM markup
  )
}
```

- What's going on here, with this function? We are telling React, to immediately run, after every render of this component.
- Whenever something updates, it will render the JSX, and then call the function inside the `useEffect()`.

- Why do you need `useEffect()`, couldn't you just put the function right in the body and have it render?

```JAVASCRIPT
// Counter.js

function Counter({ name, initialVal = 0 }) {

  // state management
  const [count, setCount] = React.useState(initialVal);

  //React.useEffect(() => {
  //  document.title = `(${count}) - Counter 2.0`;
  //})

  // Can't you just do this? Not considered good practice. 
  document.title = `(${count}) - Counter 2.0`;

  return (
    // DOM markup
  )
}
```

- Has to do with updates, and the re-render of the component. You shouldn't have to run the function everytime the component updates. In this example, only when `${count}` changes.

- The way it works, you specify a second argument as an array of dependencies.
- The first argument is the function and the second argument is the array of dependencies.

```JAVASCRIPT
// Counter.js

function Counter({ name, initialVal = 0 }) {

  // state management
  const [count, setCount] = React.useState(initialVal);

  React.useEffect(
    // first argument, function
    () => {
      document.title = `(${count}) - Counter 2.0`;
    },
    // second argument, list of dependencies
    [count]
  );
  return (
    // DOM markup
  )
}
```

- 🚀 Tells React, to only call the function, when the `count` variable changes.

- So instead of calling it on every single render, it only fires when something is updated in the second argument array, list of dependencies.
- But, the effect will always execute in the very first render. And the dependency array, will always tell React, how to re-render, when to fire the function.

### Exercises, useEffect

Get some practice with `useEffect` hook.

#### Logging a particular value

- Sign up form with several React state variables.
- Goal is to add a `console.log` that fires only when the value of "email" changes. We should see teh user's email logged in the console whenever they edit that field.

ACs:

- Whenever the user changes the value of the "email"state variable, the new value should be logged to the console.
- Nothing should be logged when the user changes their name, city or postal code.

Stretch Goal:

- Update the code so that `name` is also logged whenever it changes.

- Solution Code;
- TIP: ℹ️ You can have as many `useEffect()` hooks as you need so each only deals with a single concern.

```JAVASCRIPT
import React from 'react';

function SignupForm() {
  const [name, setName] = React.useState('');
  const [email, setEmail] = React.useState('');
  const [city, setCity] = React.useState('');
  const [postalCode, setPostalCode] = React.useState(
    ''
  );

  React.useEffect(
    // function
    () => {
      console.log({name});
    },
    // dependencies
    [name]
  );

    React.useEffect(
    // function
    () => {
      console.log({email});
    },
    // dependencies
    [email]
  );

  return (
    <form>
      <Field
        id="name"
        label="Preferred Name"
        value={name}
        onChange={(event) => {
          setName(event.target.value);
        }}
      />
      <Field
        id="email"
        type="email"
        label="Email Address"
        value={email}
        onChange={(event) => {
          setEmail(event.target.value);
        }}
      />
      <div className="row">
        <Field
          id="city"
          label="City"
          grow={2}
          value={city}
          onChange={(event) => {
            setCity(event.target.value);
          }}
        />
        <Field
          id="postal-code"
          label="Postal Code"
          grow={1}
          value={postalCode}
          onChange={(event) => {
            setPostalCode(event.target.value);
          }}
        />
      </div>
      <button>Sign up</button>
    </form>
  );
}

function Field({
  id,
  label,
  type = 'text',
  grow,
  value,
  onChange,
}) {
  return (
    <div
      className="field"
      style={{ '--grow': grow }}
    >
      <label htmlFor={id}>{label}</label>
      <input
        type={type}
        id={id}
        value={value}
        onChange={onChange}
      />
    </div>
  );
}

export default SignupForm;
```

### Exercise, Locally Persisted State

Imagine you are building an app with a "Dark Mode" toggle. It would be annoying if users had to keep toggling their preferred mode every time they load the application.

- Update the code ts that the user's preference is saved in localStorage, and restored when the page reloads.
- The current value of `isDarkMode` should be "remembered", and usd whn the page is refreshed.

- In plain JS, we can do this with the following code:

```JAVASCRIPT
// Save the value;
window.localStorage.setItem('is-dark-mode', true);

// Retrieve the value;
window.localStorage.getItem('is-dark-mode');
```

- AC's
  - The value of `isDarkMode` should be saved in localStorage whenever it changes, using the `useEffect` hook.
  - The initial value of the `isDarkMode` state variable should be retrieved from localStorage (or set to `false` if no value has been saved).
  - You can use the string `"is-dark-mode"` for the key.
  - Note: Items saved to localStorage are always saved as a string. You will need to convert the stored value back to boolean.  You can do this with the `JSON.parse()`.

- ℹ️ Check out the [Local Storage troubleshooting guide](https://courses.joshwcomeau.com/support/local-storage-troubleshooting) from Josh.

- Set into storage:

```JAVASCRIPT
function App() {
  const [isDarkMode, setIsDarkMode] = React.useState(false);

  React.useEffect(
    () => {
      // save to local storage
      window.localStorage.setItem('is-dark-mode', isDarkMode);
    },
    [isDarkMode]
  );

  return (
    // JSX here
  )
```

- Get it from storage:
- Note: `localStorage` is only capable of storing string, you have to convert it to the type you want.

```JAVASCRIPT
function App() {
  
  // get from localStorage
  const storedValue = window.localStorage.getItem('is-dark-mode');
  // convert the localStorage string to a boolean
  const parsedValue = JSON.parse(storedValue);

  // then use the parsedValue as your initial state
  const [isDarkMode, setIsDarkMode] = React.useState(parsedValue);

  React.useEffect(
    () => {
      // save to local storage
      window.localStorage.setItem('is-dark-mode', isDarkMode);
    },
    [isDarkMode]
  );

  return (
    // JSX here
  )
```

- But, need to address `null` value when nothing is specified on initial load.
- Update the `parsedValue` with an OR conditional `||` so it will default to `false`

```JAVASCRIPT
  const parsedValue = JSON.parse(storedValue) || false;
```

- One last thing, we don't want to run `window.localStorage` on every single render, it is expensive to interact with local storage.
- You can use the 'alternative form factor', whatever that means, in the `useState`. Use a callback function inside of state.
- On initial load, it will use the callback function to figure out what the value should be, but on every render after that, it won't run.

```JAVASCRIPT
function App() {

  // then use the parsedValue as your initial state
  const [isDarkMode, setIsDarkMode] = React.useState(
    // Callback function to determine the initial value on the initial render only.
    () => {
      // get from localStorage
      const storedValue = window.localStorage.getItem('is-dark-mode');
      // return the value
      return JSON.parse(storedValue) || false;
    }
  );

  React.useEffect(
    () => {
      console.log(isDarkMode);
      // save to local storage
      window.localStorage.setItem('is-dark-mode', isDarkMode);
    },
    [isDarkMode]
  );

  return (
    // JSX here
  )
```

### Effect Lint Rules

-
