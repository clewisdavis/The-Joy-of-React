# Module 2 - Working with State

- [Course Outline Notes](course-notes.md)

## Forms

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
