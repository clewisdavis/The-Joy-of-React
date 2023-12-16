# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Elements Revisited

- The fact that you reference the variable `{counterElem}` twice, doesn't matter for the component instances.
- When you define the variable, it is just a description of the UI you want.
- If you have the description twice, than that description will be used to create two different component instances.

```JAVASCRIPT
function App() {
  const counterElem = <Counter />;
  console.log(counterElem);
  
  return (
    <div>
      <p>
        {counterElem}
        {counterElem}
      </p> 
    </div>
  );
}
```

- 🤔 React elements are nothing more than descriptions of what we want to display. And the only thing that matter is if the description changes or not.

This is a very dense topic, recommend coming back to the video to re-watch.
