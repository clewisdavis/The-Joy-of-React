# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Principle of Least Privilege

The idea is that every member of the organization, only has enough access in order to do their job. Like a bank teller, need to be able to open accounts and cash checks, but does not have permissions to issue montages or open brokerage accounts. Outside the scope of that role, so the computer system should not let the employee perform those actions.

- Now imagine that every component in your application is like one of those employees. And the corporation is your application.

- In our shopping list example, the `AddNewItemForm` component has only one job, to take the value the person has entered, and add it to the list of items.

![List Shopping](images/image.png)

- If you pass the state setter function `setItems` to the component, you are giving it a LOT of control, and can have unintended consequences in large codebase.

- When you give a component so much power and privilege by passing a state setter function directly, you run the risk of accidentally break things.

- 🤔 The preferred pattern, is to wrap up your state setter functions, and add any business logic, before you hand them down to children components.

```JAVASCRIPT
import React from 'react';

import AddNewItemForm from './AddNewItemForm';

function App() {
  const [items, setItems] = React.useState([]);
  
  //   Wrap up your state setter function and business logic
  function handleAddItem(label) {
    const newItem = {
      label,
      id: Math.random(),
    };

    const nextItems = [...items, newItem];
    setItems(nextItems);
  }

  return (
    <div className="wrapper">
      <div className="list-wrapper">
        <ol className="shopping-list">
          {items.map(({ id, label }) => (
            <li key={id}>{label}</li>
          ))}
        </ol>
      </div>
      // Pass function down to child components
      <AddNewItemForm
        handleAddItem={handleAddItem}
      />
    </div>
  );
}

export default App;
```

### Exercises, Todo List Application

Let's suppose we are building a Todo app:

![To Do App](images/image-1.png)

This app has 3 main pieces of functionality:

1. Adding new todos
2. Marking a todo as complete/incomplete
3. Deleting a todo

Your mission is to refactor the code below so that none of the descendant components have more privilege than they need to accomplish these tasks.

ACs

- The `CreateNewTodo` component should only be abel to modify the state in one specific way: to add a new todo to the list
- The `TodoList` component should only be able to modify the state in two specific ways: toggling a todo between complete/incomplete, and deleting a todo.

Notes:

- Completed solution, now you have all the business logic, `handleCreateTodo`, `handleToggleTodo`, and the `handleDeleteTodo`, centralized in on place in the `App.js` component, or where ever it makes since in your app.
- So in the future, it's much easier to tell what all the different operations are within this application.  
- Vs. having it sprinkled throughout the application and hard to find.

```JAVASCRIPT
//App.js
import React from 'react';

import CreateNewTodo from './CreateNewTodo';
import TodoList from './TodoList';

function App() {
  const [todos, setTodos] = React.useState([]);

  // Create the Todo item
  function handleCreateTodo(value) {
    setTodos([
        ...todos,
        {
          value,
          id: crypto.randomUUID(),
        },
      ]
    );
  }

  // Toggle the Todo item
 function handleToggleTodo(id) {
  setTodos(todos.map(todo => {
    if (todo.id !== id) {
      return todo;
    }
    
    return {
      ...todo,
      isCompleted: !todo.isCompleted,
    }
  }));
 }

 // Delete Todo
 function handleDeleteTodo(id) {
  setTodos(todos.filter(todo => (
    todo.id !== id
  )));  
 }

  return (
    <div className="wrapper">
      <div className="list-wrapper">
        <TodoList todos={todos} handleDeleteTodo={handleDeleteTodo} handleToggleTodo={handleToggleTodo} />
      </div>
      <CreateNewTodo handleCreateTodo={handleCreateTodo} />
    </div>
  );
}

export default App;
```

- Sandbox - [Least Privilege](https://codesandbox.io/p/sandbox/least-privilege-y9tlgh?file=%2FApp.js&layout=%257B%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522rootPanelGroup%2522%253A%257B%2522direction%2522%253A%2522horizontal%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522id%2522%253A%2522ROOT_LAYOUT%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522clqxtjrzn00063b5vyypm69i2%2522%252C%2522sizes%2522%253A%255B75.70052793803619%252C24.29947206196381%255D%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522EDITOR%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522id%2522%253A%2522clqxtjrzn00023b5vvex505v5%2522%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522SHELLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522id%2522%253A%2522clqxtjrzn00033b5vm880hw95%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522DEVTOOLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522id%2522%253A%2522clqxtjrzn00053b5vg4bl7ebv%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%252C%2522sizes%2522%253A%255B50%252C50%255D%257D%252C%2522tabbedPanels%2522%253A%257B%2522clqxtjrzn00023b5vvex505v5%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clqxtjrzm00013b5vagdo858w%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522FILE%2522%252C%2522filepath%2522%253A%2522%252Findex.js%2522%252C%2522state%2522%253A%2522IDLE%2522%257D%252C%257B%2522id%2522%253A%2522clqyd0vol00023b5vl3ppm812%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522FILE%2522%252C%2522initialSelections%2522%253A%255B%257B%2522startLineNumber%2522%253A7%252C%2522startColumn%2522%253A46%252C%2522endLineNumber%2522%253A7%252C%2522endColumn%2522%253A46%257D%255D%252C%2522filepath%2522%253A%2522%252FCreateNewTodo.js%2522%252C%2522state%2522%253A%2522IDLE%2522%257D%252C%257B%2522id%2522%253A%2522clqz8mmgc00023b5vs6wl4czl%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522FILE%2522%252C%2522initialSelections%2522%253A%255B%257B%2522startLineNumber%2522%253A11%252C%2522startColumn%2522%253A18%252C%2522endLineNumber%2522%253A11%252C%2522endColumn%2522%253A18%257D%255D%252C%2522filepath%2522%253A%2522%252FApp.js%2522%252C%2522state%2522%253A%2522IDLE%2522%257D%255D%252C%2522id%2522%253A%2522clqxtjrzn00023b5vvex505v5%2522%252C%2522activeTabId%2522%253A%2522clqz8mmgc00023b5vs6wl4czl%2522%257D%252C%2522clqxtjrzn00053b5vg4bl7ebv%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clqxtjrzn00043b5vm1aqdmwa%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522UNASSIGNED_PORT%2522%252C%2522port%2522%253A0%252C%2522path%2522%253A%2522%252F%2522%257D%255D%252C%2522id%2522%253A%2522clqxtjrzn00053b5vg4bl7ebv%2522%252C%2522activeTabId%2522%253A%2522clqxtjrzn00043b5vm1aqdmwa%2522%257D%252C%2522clqxtjrzn00033b5vm880hw95%2522%253A%257B%2522tabs%2522%253A%255B%255D%252C%2522id%2522%253A%2522clqxtjrzn00033b5vm880hw95%2522%257D%257D%252C%2522showDevtools%2522%253Atrue%252C%2522showShells%2522%253Atrue%252C%2522showSidebar%2522%253Atrue%252C%2522sidebarPanelSize%2522%253A15%257D)
