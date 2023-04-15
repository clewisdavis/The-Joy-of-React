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
- All the platfoms use the same core framework, `react`, but you have to specify the correct package based on what you are building.

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
    2. The properties we want this elemetn to have
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

- The `document.querySelector` is a reference point of the exsiting DOM element.
- In your `inded.html` file, you include the `id="root"` element to the DOM element you want to mount the app. `<div id="root"></div>`

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

- With taht DOM element craeted, it adds it to the page at the specified root.
- **It takes a JS based description of some HTML, and uses it to produce real-world DOM nodes.**

- But, the real magic happens when things change.
- NOTE: React 17, used an different API syntax from the example above.

## Build your own React

- Build your own `render` funtion that takes React elements and produces the equivlent DOM struture.

- Write a `render` function that accepts a Rect element and a reference to a DOM contianer element that will hold our app.

- Acceptance criteria:
  - A link should be shown in the "result" pane, linking to a wikipedia.org, adn with the text "Read more on Wikipedia"
  - It should work with any element type (eg anchors, paragraphs, button...)
  - It shoud lhandle all HTML attributes (eg. href, id, disabled...)
  - The element should contain the text specidied uner `children`. `children` will always be a stiring.

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
- React elemetns can form a tree structure, just like HTML. This happens when we set the "children" parameter of a React element to another React element.

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

- ℹ️ The JSX we write gets converted into `React.createElement`. By the time our code is running int he user's browser, al the JSX has been zapped out, and we are left wtih a JS file, full of nested `React.createElement` calls.

- React import? Best practive to always import React, `import React from 'react';`
- In JSX, it's obfuscated, meaning it's not used by JSX, only after JSX becomes compiled into `React.createElement`

### Expression Slots

- When you want to see the result of an expression, you have to wrap it in a `{}`
- But what is actaully going on here?

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

- To add a comment in JSX, we use experssoin slot:

```JAVASCRIPT
const element = (
    <div>
     {/* some comment */}
    <div>
);
```

- Single line syntax comments everythign out, including the `{}` for the expression slot.

### Attribute expressoin slots

- We can use the same trick for dynamic attritbute values.

```JAVASCRIPT
const someIdentifier = 'some-unique-identifier';

const element = (
  <div id={someIdentifier}>
      Hello World
  </div>
);
```

- This allos su to craete expression slots for the valeu of the `id` attribute.
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

- React wil automaticaly convert types as required when supplying attributes in JSX.
- These are identical.

```JAVASCRIPT
// This works:
<input required="true" />

// And so does this
<input required={true} />
```

- In the first example, we set the requried attribute as a string `"true"`, second example, it's set as a boolean attribute `true`. Either way, the input will wind up being required.

- You can also pass numbers or strings for numberic attribute:

```JAVASCRIPT
// ✅ Valid
<input type="range" min="1" max="20" />
// ✅ Valid
<input type="range" min={1} max={20} />
```

- ℹ️ Boolean attributes:
- In HTML, it's possibel to set attributes to `true` just by specifying only the key.
- `<input required>`
- In JSX, these are equivilant:
- `<input requried />` and `<input required={true} />`
- Prefer to spell it out.

### Differences from HTML

- JSX looks like HTML, but are soem fundamental differences.

- **Reserved Words**
- In JS, has a couple of reserved words, keywords with built in functionality and we cannot use them as variable names.

- For example, `const while = 10;`, if we run this code, we get a syntax error, `while` is reserved for "while loops".

- Becuse JSX gets transformed into JS, cannot use reserved word inour JSX. This can be a problem, sometimes, HTML attrubutes sometimes overlap with oru JS reserved words.

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

- In HTML5, some elements don't ahve closing tags, the `img` tag for example.

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

- HTML is a case insensitive langauge. Many years ago, it was common to write HTML in all uppercase.

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

- JSX, is case sensiteive, our gags must all be lowercase.

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

- Other properteis that need to be 'camelCased' include:

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

- In HTML, teh style attribtue allows ut o apply some styles inline, to an element.

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

- And, React wil automatically apply the `px` suffix for certian properties.

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

- In this example, no space between the bodl text and the number. It renders `returns:123`.
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

  - A `<strong>` tag wtih some text
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
  - The label should be attached to the input; clickign the label text should focus the input
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
  - The user's avatar shoudl have helpful descriptive alt text
  - The user's name should be a link, and it should link to `/users/[username]`. With this data, it should be `/users/benjamintorn`. No actaul profile page, so link will not do anything
  - The footer should include the word "Posted" before the published date.

- HINT: You might want to start with the message content and footer, saving the header for last.

- ℹ️ Use string interpolation for the url, `${}` is how you do string interpolation. So for the url, you can do.

- Solution Code:

```JAVASCRIPT
import React from 'react';
import { createRoot } from 'react-dom/client';

const message = {
  content:
    'Just ate at “Les Corbeaux En Colère”. Wonderful little venue!',
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

- Modern JS, allows ust o embed variabels and other expressions right inside strings:

```JAVASCRIPT
const dynamicString = `hello ${userName}!`;
```

- In orde rto use strin ginterpolation, we need to use the backticks (`).
- Strings created with backtics are known as "template strings". With one super-power, can embed dynamic segments.

- We create a dynamic segemtn withinour string, by writing `${}`.
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
- What is a component exactly? **A component is a bundle of markup, styles, and logic that controls everything about a specific part of the user interfce.**
- It's a different mental model when it comes to code organization. Instead of seperating our apps into markup (HTML), styles (CSS), and logic (JS), we organize our apps into components.
- Good image reference: <https://twitter.com/areaweb>

### Mechanisms of resuse

- Traditional HTML doesn't really have a way to reuse amrkup. Some languages use `partials` to acheive this. A chunk of HTML inserted into another HTML document.

- In CSS, the main way to reuse is a `class`. A `btn` style for example.

```CSS
.btn {
  padding: 8px 32px;
  background: blue;
  border: none;
  font-size: 1rem;
}
```

- For JS, the mechanism to reuse is the `function`. For example, a function to process data in smoe way:

```JAVASCRIPT
function shout(sentence) {
  return `${sentence.toUpperCase()}!!`;
}

shout('here we go');
```

- With React, compoennta re the main mechanisme of reuse.
- Instead of partials for HTML, classes for CSS and funtions for JS, we create a compoent that bundles up all 3, and allows us to creat ea library of high-level UI elements.

- ℹ️ MOdern React also features `hooks`, which offers a way to reuse React logic. In future lessons.

### Thinking in Components

- Go through the exercices and start thinking in terms of compoennts.
- Annotate the screenshots, how would you organize them and break them down as components?
- Where would you re-use? How would you break them down.
- What can variants of the same component? One component with different instances?
- Is the difference enough to create a seperate component? Button, vs Button Icons for example.
- Think in terms of broad strokes, and the page routes / navigation.
- Then start to break down the components within each route.

### Basic Syntax

- In React, components can be defined as JS funcitons.
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
      Greetings, weary traveller!
    </p>
  );
}

const container = document.querySelector('#root');
const root = createRoot(container);
root.render(<FriendlyGreeting />);
```

- This example. `FriendlyGreeting` create a React element that descirbes a paragraph, with some built in styles.
- We render a component just like hwo we render an HTML tag. Instead of rendering a `<div>` or a `<h1>`, we render a `<FriendlyGreeting>`.

- **The big component rule**

- One must rule to follow when creating components. React components need to start with a **Capital Letter**.
- Has to do with the JSX to JS transformation.
- If our component had a lower-case funciton name, React would rednder `<friendlygreeting>` HTML elsement, instead of processing it as a component.

- **A React element is a description of a thing we want to create.**
- In some cases we want to create a DOM node, like a `<p>`. In other cases, we want to create a component instance.

### Props

- So far, our `<FriendlyGreeting />` is kinda neat, but not that useful. It renders the same thing every time. Not that flexible.

- Components use `props`. They are like arguments to a function: allow you to pass data to our components, so the components can include customizations base on teh data.

- How would you tweak our greeting component to take a person's name?
- How do you funnel data into a component. Similar to giving data to and HTML element `<div id="some-div">Hello</div>`, passing `some-div` to that HTML element.
- You do the same thing, when it comes to React compoennts, `<FriendlyGreeting name="chris" />`
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
- ℹ️ Desctucturing: To extract data directly from an object, direclty in the function decloration, or anywhere else.

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

- Let's suppose we are wroking on our `FriendlyGreeting` component. But we don't know everyone's name.

- For example, you can render a fallback value. I you know their name: `Hey Chris!`, If not, `Hey There!`

- We can do this with the `||` operator, like.

```JAVASCRIPT
funciton FriendlyGreeting({ name }) {
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

- Another example, we have a decorative "Horizonal Rule" compoennt, essentially a way to draw a line between sections. Has a default width of 100px, btu we can override that value.

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

- ℹ️ `defaultProps` property, is used by older versions of react. Don't use this property, the recommended approach is to use desctructuring assignment.

### The Children Props

- Imagine you are buidling a custom button component. It should look and act just like a regular HTML button, but have red background and white text.

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

- React allos us to do the same thing with oru custom components:

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

- React does this for us, when we pass soemthing between the open and close tags, react will automatically supply that value to use under `children`

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

- With the two `<button>` elements, Create a `Button` compoennt and use it to render both buttons.

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
- And we use this status to determint the color.

- **Solution 3: Symantic "status" prop**

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

- This way, we created some seperation between the semantic meaning of this button and the particlar color. So if you want to change things in the future, you can do it in one place.

### Application Structure

- Most React apps will share a common structure.

- An `index.js`, `App.js` and a `Header.js`
- See the [code sandbox](https://codesandbox.io/s/cehrev?file=/index.js&utm_medium=sandpack)

#### Index.js

- The root `index.js` file is the first bit of code to be executed. Responsible for rendering our React app, turning the elements we write into live DOM nodes.
- Ther ewill only be 1 spot int he entire codebase that calls the `createRoot` and `render` methods form react-dom.
- Common to do setup tasks in this file as well. Projects created with `create-react-app` include some performace, also you can include CSS files.
- Typically don't want to render a bunch of JSX, or include things like headers and buttons etc.
- General rule, just renders the single elment: `<App />`

#### App

- Common fo rour projects to have a component called `App`
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
- 🤔 Hint: Try converint the JSX to `React.createElement` function calls.

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

- 🚀 A fragment is a special React component taht doe snot product a DOM node. It looks like this:

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

- From a previous example; Building a CRM, we extracted a `ContactCard` compoennt and used it for our 3 contacts:

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
- If we are building a CRM software, this data will be dynamic. Every user will have a seperate set of contacts, and change ove rtime. We can't hardcode.

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
- 🤔 HINT: You can render an array inside the JSX, React will unpack it for you. For a refresher, you can look at "[Aray Iteration Methods](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/08-array-iteration-methods)" lesson.

- You can use the `map.()` method to interate over the data

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

- When interating with React, it is not uncommon to wind up with a structure like this:

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

#### Why are keys neceessary?

- Why can't React figure this out on it's own?
- You need keys, to help react understand exaclty how the data shifts over time.
- When updating the DOM, there are multiple ways to get from one UI to another. And if you give React unique identifiers, we can see how those things change 🤔
- [Video explanation](https://courses.joshwcomeau.com/joy-of-react/01-fundamentals/06.02-keys)

#### Using the array index as the key?

- What if you are not given a unique id for each contact, like the example above.
- A common approachc is to use the array index. The index is provided when we call the `.map()` method.

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

- This can be a bit risky, React keys are meant to be uniquely identiry a bit of content.
- In the re-order case, you could end up with something like this.

```JAVASCRIPT
<li key={0}>feed goldfish</li>
<li key={1}>buy milk</li>
<li key={2}>return library book</li>
```

- This could cause some bugs in your app.
- A good rule, is to not use `key` with array index if the items can be reordered.

#### Key Rules

- Some ruels that govern how keys should be used.

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

- When using fragments, it's sometimes requried to switch to the long form `react.Fragment`, so you can apply the key.

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
- Each `.map()` cal produces a seperate arry, and so it's not a problem.

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
  // interate over
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

- Note, Josh's solution, you can put the code inline within the return. Not make a seperate variable.

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
- Use desctructuring to simplify the return
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
- If an item is otu of stock, they cannot purchase it. Should be displayed seperately.

- Started on, but two problems remain:
  - Need to show all the items in teh user's cart, not just the first one.
  - Need to show two seperate tables. One for in-stock, and one for sold-out.

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

- you can't put an `if` statement in teh middle of a function call liek this. Here is a more simple example:

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

- In React, we are allowd to put *expressions* in our JSX, but NOT *statements*.
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
- We can assign chunks of JSX to a variable, anywhere within oru compoennt definition.

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

- The downside to useing an `if` statement is we have to pull that logic up, away from the rest of the markup.
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

- If the value it `true`, it evaluates tow hatever is on the right hand side of the operator `{isOnline && <div className="green-dot" />}`, which is the `<div>` element containing our green dot.

- 🎁 You can check out Logical Operators for JS primer. or [MDN Loginal AND &&](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND)

##### Common gatcha: the number zero

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
      {!isLoggedIn && <p>Please lonin first</p>}
    </>
  );
}
```

- This works, btu it's a bit clunky. In this case, we can use the **ternary operator**.
- 👀 MDN reference, [Conditional ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)

- Same code above, with ternary

```JAVASCRIPT
funtion App({ user }) {
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
- It rose to prominence with React, because it allows us to embed the if/else logic wihtin out JSX. Because a ternary opertor is an opertor instead fo a statement, it can be used inside JS expressions.

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

- But in most cases the perforance differences are so small, does not matter.

- Suggest: use conditional rendering by default, test on low end devices, is something seems laggy, try an alternative approach.

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

- Added another expression slot after the name, with the logical `&&` operator, if `inOnline` is true, it will return the `VisuallyHidden` componennt.

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

- Most social sites have community badges, awards for people who acheive certain goals.
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

- An alternative approach is the `range` funtion.
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

##### Range funtion code

- Code for the `range` function.
- You can either inlude this in your `utils.js` or load from [lodash](https://lodash.com/docs/4.17.15#range).

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
- 2. There shold be X divs with a class of `row`, where X is equal to the `numRows` prop.
- 3. Inside each `row`, there should be Y divs with a class of `cell`, where Y is equal to the `numCols` prop.
- 4. You should use the provided `range` function to solve this problem.
- 5. There shouldn't be any key-related warnings in the console. Youc an assume that the grid is "static", with no plans to support reordering the rows or cells.

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

- React is unpoinionated when it comes to styling. As a results, dozens of options when it comes to using CSS with React.

- Technically, you can do things the traditional way. Bunch of CSS files and incude them as a `<link>` in your `index.html`.

- 🤔 Thinking in Components
- The core idea with components is taht each compoent is a bundle of markup (in JSX)
  - Logic in JS
  - Styles in CSS
  - Markup in HTML

- For example, our `button` component should "own" all the the styles related to that component.
- Working with CSS becomes much simpler.

#### CSS Modules

- Let' build a component, called "Sidenote", a callout to hold tangential information.
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
- Also, note how super short and generic, `.wrapper` for example is typically used througout an application. A top level element.
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
- 🚀 This prefix, is what **gaurantees the uniqueness**.
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
  - And producing the `styles` object, so we can use the styles in our compoennt `className={styles.wrapper}`

- We get all the benefits of the naming convention BEM, without having to manually do it. All the hard stuff just happens.
- How is the prefix defined? It is using the file path, and converting it into the prefix. `/components/Sidenote.module.css` gets converted to the prefix for each style. `_components_Sidenote_module__wrapper`
- **You will never have a collision, because you can never have two files with the same name.**

- Other tools do the same thing, like `styled compoennts` but has a learning curve.
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

- In this exerciese, you will iimipelemnt this pattern using CSS modules. The task is to apply the correct class name base on the `type` prop.

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

- To add multiple classes, use a template string literal to combine, concatanate.
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
- When we use square brackets, we create an "expression slot". We can put any JS expression we want in there, and it will resolve that expressoin to a key.

- Dot notation has become the standard way of doing things if we know the exact name of they key we want to loook up.
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
- I used the Logical AND `&&` opertator on the rating to display the class. Then passed that into an expression slot.

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

- ℹ️ Class utilities: The `className` prop expects a string, this string can include multiple classes, seperated by spaces.
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

## Working with State

- In the early days of the web, websites were basically fancy documents, we would load one HTML file, read the content and then load another one.

- The modern web is interactive. The app can respond to user actions in real time. Without the need to fetch the whole page.

- In this module:
  - How to respond to user acitons with event binding.
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

- In order to respond to an event, we need to listent for it. JS already provides a built in way to do this, `addEventListener` method:

```JAVASCRIPT
cosnt button = document.querySelector('.btn');

function doSomething() {
  // stuff here
}

button.addEventListener('click', doSomething);
```

- In the code above, we are listening for a specific event `click` targeting a specific element `btn`. We have a function to handle this event `doSomething`.
- When the user cicks on this particular button, your handler function will be invoked, allowing us to do something in response.

- The web platform offers another way to do this. We can embed our hadler right in the HTML

```HTML
<button onclick="doSomething()">
  Click me!
</button>
```

- Rect piggybacks on this pattern, allowing us to pass an event handler right into the JSX:

```JAVASCRIPT
funtion App() {
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

- **This is the recommeded way to handle events in React.**
- Try and use the "onX" props like `onClick` and `onChange` when possible.

- Few reasons why:
  - **Automatic cleanup:** When we use an event listener, we supposed to remove it when we are done, with `removeEventListener`. If we forget to do this, we introduce a memory leak. React automatically removes listners when we use 'on X' handler functions.
  - **Improved performance:** React can optimize thigns for use, like batching multiple event listeners together to reduce the memory consumption.
  - **No DOM interaction:** Avoid interacting with the DOM directly.

- ℹ️ One of the core ideas behind React, is that is does the DOM manipulation for you. Stay within React's abstraction rather than trying to compete with it to manage the DOM.

#### Differences from HTML, Events

- Looking at the `onChange` prop, it looks very similar tot he in HTML method of adding event handlers.

##### Camel Casing

- In JSX, you have to write `camelCased` attribute names in JSX.
- Be careful to write `onClick` instead of `onclick`. Other examples, `onChange`, `onKeyDown`, `onTransitionEnd` etc.
- If you forget, React will let you know.

##### Passing a function reference

- When working wiht event handlers in React, we need to pass a reference to the function. We don't call the function like we do in HTML:

```HTML
// ✅ We want to do this:
<button onClick={doSomething} />

// 🚫 Not this:
<button onClick={doSomething()} />
```

- When we include the parentheses, we invoke th function right away, the moment the React app is rendered. We want to give React a reference to the function, so React can call it at a later time, when the user clicks on the buton.

#### Specifying Arguments

- Here is the thing, what if we want to specify an arguemnt to a function?

- For example, if you wanted to make a function called `setTheme`, and we use it to change the user's color theme from Light Mode, to Dark Mode.

- To do this, we need to supplyt he name of the theme we are switching to, like this:

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

- We can us the `window.aletr()` to show the message.
- AC's
  - When the user click on the ball, a winning message should be shown.
  - You should handle 'click' events speficically, as this even is triggered on click, on tap, or even when a user focuses on teh elemetn with the keyboard and hits "Enter" Key
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
- ℹ️ You can write as a seperate function, especially is you have multiple expressions inside your function.

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

- You mission, if you choose it, it to fix the code so that i thows the right message depending on which item is clicked.

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
- This funtion takes a single argument: the initial value. In this case, that value is `0`. The value is chosen because when the page first loads, we clicked the button 0 times.

- `useState` is a **hook**. A hook is a special type of function that allows us to 'hook into' React internals.

- The `useState` hook returns an array containing two items:

1. The current value of the state variable. We've decided to call it `count`, this can be whatever you want to name it.
2. A function we can use to update the state variable. We named it `setCount`. It's best practice to pre-pend the `set` to the function.

- ℹ️ Destructing Assignment, below is the same thing as the above `const [count, setCount]` we are descructuring `count` and `setCount`.

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

- `user` The first desctuctured variable si the name of the thing we are tracking.
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

- We can also supply a function. React will cal lthis function on the very first render to valculate the initial value:

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

- The benefit here is taht we are only doing the expensive work (redon from localStorage) once, on the intial render, rather than doing it on every single render.

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
- Everytime we call the `run` function, we are creating a breand new `sayHi` function, but never calling it.

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

- In React, however the term "render" means somethign slighly different. I think so much of the confusion in React comes from this misunderstanding.

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
- Now, let's suppose we re-render this compoennt, and wind up with the following before/after pair of snapshots:

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
- 💡 If we spot a difference between snapshots, React will need to update the DOM, but it will be a pricisely-targeted minimal change. 💡

- When React deos cahnge a part of the DOM, teh browser will need to re-paint. A re-paint is when the pixels on the screen are re-drawn because a part of the DOM was mutated.

- This is done natively by the browser when teh DOM is edited with JS, whether by React, Angular, jQuery, vanilla JS, etc.

To Summerize: 🚀 React Re-Rendering

- A **re-render** is a React process where we figure out what needs to change, AKA reconciliation, to spot the difference between snapshots.

- If somethign has changed betweent eh two snapshots, React will "commit" those changes by deiting the DOM, so that it matches the latest snapshot.

- Whenever a SOM node is edited, the browser will re-paint, re-drawing the relevant pixel so that the user sees the correct UI.

- **No all re-renders require re-paints!** If nothing has changed between snapshots, React won't edit any DOM nodes, and nothing will be re-painted.

- 👀 The critical thing to understand si taht when we talk about re-rendering, we are not sayign that we should throw away the current UI and re-biuld everythign from scratch.

- React ties to keep the re-painting to a minimum, because re-painting is slow. Instead of generating a bunch of new DOM notes from scratch (lots of painting), it figures out what's changed between snapshots and make the requried tweaks with surgical precision.

- For more in-depth; see react docs [Render and Commit](https://react.dev/learn/render-and-commit)

#### Asynchronous Updates

- Consider the following code: What value woud you expect to see in the developer console when the user clicks the button for the first time?

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
- React does not immediately drop everything; it watis until the current operation is completed (processing the click), and then updates the value and triggers a re-render.

- For now, the thing to remember is that **updating a state variable is asynchronous**. It affects what the state will be for the **next render**. It's a scheduled update.

- Here is how to fix the code, so that we have access to the new value right away:

```JAVASCRIPT
cuntion App() {
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
- When React sees `setStrength(strength + 1);`, React makes a "calendar appointmet" but *it doesn't interupt what it is already doing*, running through the function.
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

-
