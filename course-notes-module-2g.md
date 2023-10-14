# Module 2 - Working with State

- [Course Outline Notes](course-notes.md)

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
