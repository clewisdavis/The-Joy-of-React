# The Joy of React - Module 1 - Course Notes

- [Course Outline Notes](course-notes.md)

Course notes from clewisdavis, for future reference.

- Start date February 2023

## Module 1, React Fundamentals  

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

### Productive Failure

- A learning method used from academic.
- n Productive Failure, the conventional instruction process is reversed so that learners attempt to solve challenging problems ahead of receiving explicit instruction. While students often fail to produce satisfactory solutions (hence “Failure”), these attempts help learners encode key features and learn better from subsequent instruction (hence “Productive”).
- Much more likely to remember things, if you fail first, not given all the instructions ahead of time.
- Struggling means you are learning, and growing.

- The growth mindset, we become smarter through practice, and failure is the fastest way to learn.
