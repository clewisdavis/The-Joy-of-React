# The Joy of React - Module 3 - Course Notes

- [Course Outline Notes](../course-notes.md)

## React Hooks

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
