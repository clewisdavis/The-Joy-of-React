# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Immer

One of the cardinal rules in React is that state is immutable. That is true whether we are talking about `useState` or `useReducer`.

More complex state is, the harder it is to make necessary changes without mutating anything.

Example, let's suppose you have a calendar app, and we have a array of `events`. It might look something like this:

```JAVASCRIPT
const [events, setEvents] = React.useState([
  {
    eventId: 'coffee-with-samantha',
    date: '2023-01-01T12:30:00.000Z',
    metadata: {
      invitees: [
        {
          name: 'Samantha',
          email: 'samboombox123@aol.com',
        },
      ],
    },
  },
  {
    eventId: 'focus-time',
    date: '2023-01-01T15:00:00.000Z',
    metadata: {
      notes: 'Time for me to focus!',
    },
  },
  {
    eventId: 'team-meeting',
    date: '2023-01-02T10:00:00.000Z',
    metadata: {
      notes: 'Weekly team catch-up call!',
      invitees: [
        {
          name: 'Sadb Fabian',
          email: 'sfabian@widgetco.com',
        },
        {
          name: 'Gerarda Nicomedes',
          email: 'gnicomedes@widgetco.com',
        },
        {
          name: 'Sagit Edvaldo',
          email: 'sedvaldo@widgetco.com',
        },
        {
          name: 'Denis Seppo',
          email: 'dseppo@widgetco.com',
        },
      ],
    },
  },
]);
```

- How would you remove one contact, one of the invitees without mutating any of the arrays or objects held in React state?

- Solution Code:

```JAVASCRIPT
// updateStage.js
function updateState(currentState) {
  // Pluck out some variables, to help us later on.
  const relevantEvent = currentState[2];
  const { invitees } = relevantEvent.metadata;

  // Create a new list of invitees using `filter`.
  // It should include all teammates except Gerarda.
  const nextInvitees = invitees.filter(
    invitee => invitee.email !== 'gnicomedes@widgetco.com'
  );

  // Create a brand-new team-meeting event, with
  // all the same stuff, but with this new list
  // of invitees.
  const nextEvent = {
    ...relevantEvent,
    metadata: {
      ...relevantEvent.metadata,
      invitees: nextInvitees,
    }
  };

  // Create a brand-new list of events, including
  // the first two unchanged events, as well as our
  // brand-new team-meeting event.
  return [
    ...currentState.slice(0, 2),
    nextEvent,
  ];
}
```

- [Playground - Immer](https://codesandbox.io/p/sandbox/immer-hznfh8?file=%2FupdateState.js%3A38%2C5&layout=%257B%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522rootPanelGroup%2522%253A%257B%2522direction%2522%253A%2522horizontal%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522id%2522%253A%2522ROOT_LAYOUT%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522clr4yafu500063b5wkfyousqn%2522%252C%2522sizes%2522%253A%255B70%252C30%255D%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522EDITOR%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522id%2522%253A%2522clr4yafu500023b5wk7wg01wv%2522%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522SHELLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522id%2522%253A%2522clr4yafu500033b5w6x6vjauw%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522DEVTOOLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522id%2522%253A%2522clr4yafu500053b5wuwa338k2%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%252C%2522sizes%2522%253A%255B40%252C60%255D%257D%252C%2522tabbedPanels%2522%253A%257B%2522clr4yafu500023b5wk7wg01wv%2522%253A%257B%2522id%2522%253A%2522clr4yafu500023b5wk7wg01wv%2522%252C%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clr4yrpp200023b5v2152f9lc%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522FILE%2522%252C%2522initialSelections%2522%253A%255B%257B%2522startLineNumber%2522%253A38%252C%2522startColumn%2522%253A5%252C%2522endLineNumber%2522%253A38%252C%2522endColumn%2522%253A5%257D%255D%252C%2522filepath%2522%253A%2522%252FupdateState.js%2522%252C%2522state%2522%253A%2522IDLE%2522%257D%255D%252C%2522activeTabId%2522%253A%2522clr4yrpp200023b5v2152f9lc%2522%257D%252C%2522clr4yafu500053b5wuwa338k2%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clr4yafu500043b5wh1elo9ek%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522UNASSIGNED_PORT%2522%252C%2522port%2522%253A0%252C%2522path%2522%253A%2522%252F%2522%257D%255D%252C%2522id%2522%253A%2522clr4yafu500053b5wuwa338k2%2522%252C%2522activeTabId%2522%253A%2522clr4yafu500043b5wh1elo9ek%2522%257D%252C%2522clr4yafu500033b5w6x6vjauw%2522%253A%257B%2522tabs%2522%253A%255B%255D%252C%2522id%2522%253A%2522clr4yafu500033b5w6x6vjauw%2522%257D%257D%252C%2522showDevtools%2522%253Atrue%252C%2522showShells%2522%253Atrue%252C%2522showSidebar%2522%253Atrue%252C%2522sidebarPanelSize%2522%253A15%257D)

- That is a lot of code, because we cannot mutate anything. This is where Immer comes into play.

### Immer 101

Immer is an NPM package, to make updates more manageable. What it does, allows us to write code that looks like it mutates the data. using some modern JS trickery, however, the data is never actually mutated.

For example; here is how we would solve the calendar problem using Immer:

```JAVASCRIPT
import { produce } from 'immer';

function updateState(currentState) {
  return produce(currentState, (draftState) => {
    draftState[2].metadata.invitees.splice(1, 1);
  });
}

export default updateState;
```

How does this work?

## A working draft

The `produce` function we get from Immer takes two arguments:

- The state we would like to edit `currentState`
- A callback function `(draftState) => {}`

`draftState` is a special 'wrapped' version of the `currentState`. I like to think of it as a shielded version: Immer is the guardian, and will make sure that the original object is never mutated, no matter what we try to do to this wrapped version.

After running the code in our calback function, `produce` will resolve to a brand-new object, with al the modifications applied.

A more complete example, with the `useState` hook:

```JAVASCRIPT
import React from 'react';
import { produce } from 'immer';

function App() {
  const [numbers, setNumbers] = React.useState([0, 1, 2]);
  
  function handleClick() {
    const nextState = produce(numbers, (draftState) => {
      const nextNumber = numbers.length;
      draftState.push(nextNumber);
    });
    
    setNumbers(nextState);
  }
  
  return (
    <>
      <h2>Data contents:</h2>
      <div className="items">
        {JSON.stringify(numbers)}
      </div>
      
      <div className="actions">
        <button onClick={handleClick}>
          Add next number
        </button>
      </div>
    </>
  );
}

export default App;
```

- Sandbox Example - [Immer 101](https://codesandbox.io/p/sandbox/modest-almeida-724v8p?layout=%257B%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522rootPanelGroup%2522%253A%257B%2522direction%2522%253A%2522horizontal%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522id%2522%253A%2522ROOT_LAYOUT%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522clr6dcwfw00063b5vb5af8dkq%2522%252C%2522sizes%2522%253A%255B70%252C30%255D%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522EDITOR%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522id%2522%253A%2522clr6dcwfw00023b5vmc30s73i%2522%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522SHELLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522id%2522%253A%2522clr6dcwfw00033b5vodicucu5%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522DEVTOOLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522id%2522%253A%2522clr6dcwfw00053b5vd9rnuufl%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%252C%2522sizes%2522%253A%255B50%252C50%255D%257D%252C%2522tabbedPanels%2522%253A%257B%2522clr6dcwfw00023b5vmc30s73i%2522%253A%257B%2522id%2522%253A%2522clr6dcwfw00023b5vmc30s73i%2522%252C%2522tabs%2522%253A%255B%255D%257D%252C%2522clr6dcwfw00053b5vd9rnuufl%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clr6dcwfw00043b5v84z3k2o3%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522UNASSIGNED_PORT%2522%252C%2522port%2522%253A0%252C%2522path%2522%253A%2522%252F%2522%257D%255D%252C%2522id%2522%253A%2522clr6dcwfw00053b5vd9rnuufl%2522%252C%2522activeTabId%2522%253A%2522clr6dcwfw00043b5v84z3k2o3%2522%257D%252C%2522clr6dcwfw00033b5vodicucu5%2522%253A%257B%2522tabs%2522%253A%255B%255D%252C%2522id%2522%253A%2522clr6dcwfw00033b5vodicucu5%2522%257D%257D%252C%2522showDevtools%2522%253Atrue%252C%2522showShells%2522%253Atrue%252C%2522showSidebar%2522%253Atrue%252C%2522sidebarPanelSize%2522%253A15%257D)

- This feels like cheating, but we are not actually mutating the `numbers` array held in React state.

### Performance

- Immer doesn't do a deep copy which could have poor performance, it uses a technique called 'structural sharing', and made possible using Proxies.
- There is a cost, but no where near the cost of doing a deep copy.

- Code Example:

```JAVASCRIPT
const state = {
    customer: {
        name: 'Daria Hamkin',
    },
    toppings: {
        pepperoni: true,
        anchovies: false,
        kale: true,
    },
};

const nextState = produce(state, (draftState) => {
    draftState.toppings.pepperoni = false;
});
```

- `draftState` is a proxy wrapped version of `state`. When we try to change the value of `draftState.toppings.pepperoni`, the Proxy jumps in the path of the ball, deflecting it, and replacing it with an immutable operation, like:

```JAVASCRIPT
const newState = {
    ...state,
    toppings: {
        ...state.toppings,
        pepperoni: false,
    },
}
```

- Notice the `customer` object is reused. It gets spread into the `newState` object. If it was making a deep copy, everything would be reconstructed from scratch, but thanks to 'structural sharing' and Proxies, only reconstruct the part that change.

### Drawbacks

Every tool has it's drawbacks and tradeoffs. The biggest issue is that proxies cannot easily be logged. Trying `console.log` produces some pretty strange results.

However, Immer ships with a tool, `current` which can help unpack a proxy, for debugging purposes.

```JAVASCRIPT
import { produce, current } from 'immer';

const arr = [1, 2, 3];
produce(arr, (draftArr) => {
  draftArr.push(4);

  console.log(current(draftArr));
  // [1, 2, 3, 4]
});
```

- You should never use `current` in your final production code. It's purely a tool to debug, when you are solving the problem at hand.

## Exercises

Get some practice with Immer.

### Gradient Generator

Update the Gradient Generator, which uses `useReducer`. Update the code to use Immer.

ACs:

- You should use the `produce` function form Immer to produce the new state, within the `reducer` function.
- Tweak the state-updating logic to edit the draft state using mutation, instead of returning a new state object.
- The import has already been provided for you, just under the `React` import.

- Solution Code: Exercise, [Gradient Generator](https://courses.joshwcomeau.com/joy-of-react/05-happy-practices/08.02-immer-exercises)

```JAVASCRIPT
function reducer(state, action) {
  return produce(state, (draftState) => {
    switch (action.type) {
      case 'add-color': {
        draftState.numOfVisibleColors += 1;
        break;
      }
  
      case 'remove-color': {
        draftState.numOfVisibleColors -= 1;
        break;
      }
  
      case 'change-color': {
        draftState.colors[action.index] = action.value;
        break;
      }
    }    
  });
}
```

### Exercise, Todo List

Let's update our 'Todo List' app to use Immer.

ACs:

- The reducer should be updated so that Immer is used to update the state
- Go through each action, and see if we can simplify the state-updating logic using mutation. It's up to you to decide what will work best in each situation.
- Feel free to edit things beyong the reducer, eg. to change which data gest passed through in the action

- Solution Code, updated to use Immer

```JAVASCRIPT
// App.js
function reducer(todos, action) {
  return produce(todos, (draftState) => {
    switch (action.type) {
      case 'create-todo': {
        // Push the new object in
        draftState.push(
          {
            value: action.value,
            id: crypto.randomUUID(),
          },
        );
        break;
      }

      case 'toggle-todo': {
        draftState[action.index].isCompleted = 
          !draftState[action.index].isCompleted;
        break;
      }
      case 'delete-todo': {
        draftState.splice(action.index, 1);
        break;
      }
    }
  });
}
```

### Exercices, Pizza Toppings

In this exercise, you will be working on a pizza ordering form. All of the UI is done, but there isn't any state management yet. Your mission is to manage the state using `useReducer` and Immer.

ACs:

- When the user submits the form, a `window.alert` shoudl show us what size and toppings they have selecte.
- The radio buttons and checkboxes should be controleld by the reducer's state.
- The 'Select All' button should add all of the toppings.
  - If al the topings are selected, hoever, the button label should flip to 'Remove All' and it should toggle all of the toppings off.

- This is a challenging exercise. You will need to figureout how to bing the values of checkboxes/radio button to reducer state, which is not something covered. The 'Input Cheatsheet' should get you most of the way there, but you wil need to do some playing around.

- Solution Notes:

- Write up the boilerplate for the reducer.

```JAVASCRIPT
// Write out the boilerplate scaffolding
  const INITIAL_STATE = {
    
}

// reducer boilderplate
function reducer(state, action) {
  return produce(state, (draftState) => {
    switch (action.type) {
        
    }
  })
}

function OrderPizza() {
    const [state, dispatch] = React.useReducer(reducer, INITIAL_STATE);
    // rest of component here
}
```

- How do you drive the state from the radio buttons to the state in the `reducer`? It's similar to the way you create a controlled form element.
- Add the `checked` property, `checked={state.size === slug}`
- Also add `onChange` event to the radio, and the only way to change state that lives in a `reducer` is to `dispatch()` an action.

```JAVASCRIPT
  onChange={() => {
    dispatch({
      type: 'select-size',
      slug,
    })
  }}
```

- To test this out, add to your `alert`, `window.alert(JSON.stringify(state));`

- Now, how to deal with the toppings, checkboxes?

- Add the actions for `add-topping` and `remove-topping`

```JAVASCRIPT
function reducer(state, action) {
  return produce(state, (draftState) => {
    switch (action.type) {
        case 'select-size': {
          draftState.size = action.slug;
          break;
        }
        case 'add-topping': {
          // add a key for the topping
          draftState.toppings[action.slug] = true;
          break;
        }
        case 'remove-topping': {
          // delete object from the property
          delete draftState.toppings[action.slug];
          break;
        }
    }
  })
}
```

- Then wire it up in the checkbox for element with the `checked` and `onChange`. And `dispatch`

```JAVASCRIPT
<input
      id={inputId}
      type="checkbox"
      checked={state.toppings[slug]}
      onChange={() => {
        dispatch({
          type: 'add-topping',
          slug,
        })
      }}
    />
```

- Add a condition, ternary, for the remove to the `dispatch`. This is getting confusing at this point.

```JAVASCRIPT
  // Add a variable for the topping
  // the !! resolves the intial value
  const hasTopping = !!state.toppings[slug];

onChange={() => {
    dispatch({
      // add a ternary condition to check toppings
      type: hasTopping 
        ? 'remove-topping' 
        : 'add-topping',
      slug,
    })
  }}
```

- 🤔 Really getting over my head with these exercises. May need to go back over.
