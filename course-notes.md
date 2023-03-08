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
