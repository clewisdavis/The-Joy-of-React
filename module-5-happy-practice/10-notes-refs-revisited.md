# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Refs Revisited

Earlier in the course, we built a custom useIsOnscreen hook. But something strange about it.

- Notes here, `refs` can be mutated, so think about it like a memory stick, RAM. A single space on the memory, it's not creating a new spot in memory every time the `ref` is updates.
- It's updating the same spot in memory.

![Memory](images/image-2.png)

- When you capture the `ref` from React, it edits that spot in memory.

```JAVASCRIPT
elementRef: {
    current: undefined
}
```

- Imagine a update to the `ref`

```JAVASCRIPT
elementRef: {
    current: <div>
}
```

- We are editing this object held in memory and any variable that references that object `elementRef.current` will always have access to whatever the current value is. Because the variable references this thing being held in memory.

- 🤔 Another example

```JAVASCRIPT
const person = { name: undefined };

console.log(person.name); // undefined
```

- now if you define `person.name = 'Josh'`

```JAVASCRIPT
const person = { name: undefined };

person.name = 'Josh'

console.log(person.name); // Josh
```

- now, if you create a new variable, what would you expect to log? undefined or Josh?

```JAVASCRIPT
const person = { name: undefined };

const myName = person.name;

person.name = 'Josh'

console.log(myName); // undefined
```

- Run this code and it will log `undefined`.
- The reason for this is because, you set `myName` variable is equal to `person.name` which is `undefined`. You plucked this value out, storing it as a `const` and it cannot change.

- What if you update `myName` to be just equal to `person

```JAVASCRIPT
const person = { name: undefined };

const myName = person;

person.name = 'Josh'

console.log(myName.name); // Josh
```

- Now it logs Josh, but in this case, 📣 the `myName` variable is assigned to the entire object.
- 🤔 And any variable that references that object, will be able to access that new value.

😓 whew
