# Module 1 - Components

- [Course Outline Notes](course-notes.md)

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

- ℹ️ Modern React also features `hooks`, which offers a way to reuse React logic. In future lessons.

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
