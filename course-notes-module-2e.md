# Module 2 - Working with State

- [Course Outline Notes](course-notes.md)

## Complex State

- So far we have seen how store numbers and strings in React state. Most times, our state will be in a more complex shape, ike an object or an array.

- For example; in the previous exercise, the gradient generator tool where users select 2 to 5 colors, and use state to generate a gradient.
- How to you keep track of the state for the colors?

- In this case, the `colors` state variable, hold an array of strings:

```JAVASCRIPT
  const [colors, setColors] = React.useState(['#FFD500', '#FF0040']);
```

- React doesn't care what type out state is. We can store numbers or strings or arrays or objects. We can even store functions in state if we want!

- 👀 But there's a catch, React state changes have to be immutable.
- When we call `setColors`, we need to provide a brand new array.
- **We should never mutate arrays or objects help in state**

- Immutability is a big topic in JS. You can see the lesson in the JS primer, [Assignment vs. Mutation](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/11-assignment-vs-mutation)

### Mutation Bugs

- Video summary:
- In our color picker example, we are unable to choose the colors.
- The problem, when we use complex state, with arrays or objects, we have to specify a new array.
- Otherwise, React doesn't think we changed anything.
**- React uses, reference equality as a way to check if something has changed.**
- React will check, is the object or array you are passing me, the same entity, the same reference, if it is, React does not re-render. React figures it is intentional.

- Refresher: equality, objects, memory, pointers, really important to have a firm grasp on these concepts.
- For this exercise, startup a node console to explore these concepts. New terminal, `node`

- Create a new array

```JAVASCRIPT
  let arr = [1, 2, 3, 4]
```

- Then create a new variable called `nextArr` and assign it to `arr`

```JAVASCRIPT
  let nextArr = arr;
```

- Then, take `nextArr` and modify the first value to be equal to 5

```JAVASCRIPT
  nextArr[0] = 5
```

- Now, the question is, what is the value of `arr`? You might think, the value is the same, because you didn't change `arr`, you changed `nextArr`
- But when you check the value of `arr`, is has the new initial value of 5. `[5, 2, 3, 4]`
- In fact, `nextArr === arr` is `true`, exactly equal to each other.
- What is going on here in JS? We don't have to think about the high level memory storage. In JS, whenever we write a variable, it makes a physical change in the computer, stores it in RAM.
- And the variable is just a pointer to that.

- For example; our `arr` variable, just points to this array held in memory.
- And when you say, `let nextArr = arr;`, we are not creating a new array, you are just pointing `arr` and `nextArr` to the same thing, same array.
- Both variables point to the same entity that exist in memory.
- When we change an item in `nextArr`, we are changing the same item, we are not creating a clone of it.

- If you take two arrays, with the exact same value, are they equal?

```JAVASCRIPT
  [1, 2, 3] === [1, 2, 3]
  // false
```

- This is `false` because you have two separate arrays.
**- The way JS works, you have objects in memory, and variables that point to them**

- What if you wanted to create a copy with the same values?
- You can use the spread operator `...`, this will copy the contents of the array.

```JAVASCRIPT
  let betterCloneArr = [...arr]
  // [ 5, 2, 3, 4 ]
```

- This works not matter how many values you have, it is unpacking the array into the new `betterCloneArr` variable.
- And it's creating a new array
- Now notice, `betterCloneArr` is not equal to `arr`

```JAVASCRIPT
  betterCloneArr === arr
  // false
```

- 👀 JS Primer on the [Spread operator](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/12-rest-spread)
- And a good blog post, [visual reference to JS](https://daveceddia.com/javascript-references/)

- In our color picker example, all that matters is that React gets a new object and does the re-render.

### Never mutate React state (even when it seems to work)

- In the example, we follow these steps

1. Create a new array
2. Modify that new array
3. Set the new array into state

- Is it OK to flip the order of the first two steps? What is we modify the array, then clone it? Like this:

```JAVASCRIPT
  <input
  onChange={event => {
    // Mutate the array:
    colors[index] = event.target.value;

    // Create a new copy, and set it into state:
    setColors([...colors]);
  }}
/>
```

- Seems simpler, right? Make whatever modifications we want, and then copy the array right before we call `setColors`, so that we are providing a new value.

- **Here is the problem**. By doing it this way, we are modifying the values held in React state.
- React really doesn't expect us to do this, and has no guardrails in place to warn us if we try.

- You might get lucky and get away with it once, but if you make a habit of it, you will likely start getting weird / glitchy behavior.
- A random part fo the page won't be updated when supposed to. Or maybe the DOM structure will get shuffled, putting elements in different parts of the DOM.

- These bugs are really hard to diagnose and fix, you will save yourself a whole lot of trouble if you do your best to avoid mutating React state.

### But isn't' this inefficient?  

- To create brand new arrays on every single change, isn't that inefficient? Wouldn't it be more performant to mutate the existing array instead?
- When it comes to React state, these is no choice; we need to produce a new array whenever the state changes.

- Fortunately, this isn't an issue. Cloning an array is an incredibly quick operation.

- In our color gradient picker example, when we clone the `[...colors]` array, it takes about 2 microseconds.
- That's two millionths of a second. For context, the average human blink is 100 milliseconds; we could clone 50,000 arrays in the blink of an eye.

### Exercises, Complex State

- Working on the gradient generator tool! Instead of limiting the user to 2 colors, let's ad up to 5 colors.

- AC's
  - The color inputs should work; picking a new color should update the gradient accordingly.
  - Clicking "Add color" should add a new color, at the end of the array.
  - Clicking "Remove color" should remove the last color in the array.
  - When adding new colors, they should default to `FF0000` bright red.
  - There should always be between 2 and 5 colors. No more, no less. The user should be given feedback as to why the buttons stop working once they hit a limit.

- Starter code, gradient picker

```JAVASCRIPT
// App.js
import React from 'react';
function App() {
  const [colors, setColors] = React.useState([
    '#FFD500',
    '#FF0040',
  ]);
  
  const colorStops = colors.join(', ');
  const backgroundImage = `linear-gradient(${colorStops})`;

  return (
    <div className="wrapper">
      <div className="actions">
        <button>
          Remove color
        </button>
        <button>
          Add color
        </button>
      </div>
      
      <div
        className="gradient-preview"
        style={{
          backgroundImage,
        }}
      />
      
      <div className="colors">
        {colors.map((color, index) => {
          const colorId = `color-${index}`;
          return (
            <div key={colorId} className="color-wrapper">
              <label htmlFor={colorId}>
                Color {index + 1}:
              </label>
              <div className="input-wrapper">
                <input
                  id={colorId}
                  type="color"
                  value={color}
                  onChange={(element) => {
                      // create a new array
                      const nextColors = [...colors];
                      // mutate it
                      nextColors[index] = event.target.value;
                      // add to state
                      setColors(nextColors);
                      console.log(colors);
                  }}
                />
              </div>
            </div>
          );
        })}
      </div>
    </div>
  );
}

export default App;
```

- AC: The color inputs should work; picking a new color should update the gradient accordingly.

```JAVASCRIPT
<div className="input-wrapper">
    <input
      id={colorId}
      type="color"
      // add a value to make a controlled
      value={color}
      onChange={(element) => {
          // create a new array
          const nextColors = [...colors];
          // mutate it
          nextColors[index] = event.target.value;
          // add to state
          setColors(nextColors);
          console.log(colors);
  />
</div>
```

- AC: Clicking "Add color" should add a new color, at the end of the array.
- Tip 🤔 Don't forget the rule about 'Never Mutate React State',
  - 1. Create a new array
  - 2. Modify that new array
  - 3. Set that new array into state

```JAVASCRIPT
   <button
     onClick={(event) => {
       console.log(event.target)
       // create a new array
       let newColors = [...colors]
       // modify that array, set default to red
       newColors.push("#FF0000")
       // set it to state
       setColors(newColors)
       console.log(colors)
     }}
   >
     Add color
   </button>
```

- AC: Clicking "Remove color" should remove the last color in the array. Don't forget about about the react mutate rule.

```JAVASCRIPT
  <button
    onClick={(event) => {
      console.log(event.target)
      // 1. create a new array
      let removeColor = [...colors]
      // 2. modify that array
      removeColor.pop()
      // 3. set to state
      setColors(removeColor)
      console.log(`new colors ${colors}`)
    }}
  >
    Remove color
  </button>
```

- AC: When adding new colors, they should default to `#FF000` (bright red).

```JAVASCRIPT
       // modify that array, set default to red
       newColors.push("#FF0000")
```

- AC: There should always be between 2 and 5 colors. No more, no less. The user should be given feedback as to why the buttons stop working once they hit a limit.

```JAVASCRIPT
// my attempt
// get the number or colors from the colors array
// if 2 colors, disable the remove button, throw an alert
// if 5 colors, disable the add button, throw an alert
```

```JAVASCRIPT
import React from 'react';
function App() {
  const [colors, setColors] = React.useState([
    '#FFD500',
    '#FF0040',
  ]);
  
  const colorStops = colors.join(', ');
  const backgroundImage = `linear-gradient(${colorStops})`;

  // function for the minimum
  function colorMin(param) {
    console.log(param)
    param < 2 ? alert('min of 2') : undefined
  }

  // function for the max
  function colorMax(param) {
    console.log(param)
    param > 5 ? alert('max of 5') : undefined
  }

  return (
    <div className="wrapper">
      <div className="actions">
        <button
          onClick={(event) => {
            console.log(event.target)
            // 1. create a new array
            let removeColor = [...colors]
            // 2. modify that array
            removeColor.pop()
            // 3. set to state
            setColors(removeColor)
            console.log(`new colors ${colors}`)
            // color min
            colorMin(removeColor.length)
          }}
        >
          Remove color
        </button>
        <button
          onClick={(event) => {
            console.log(event.target)
            // 1. create a new array
            let newColors = [...colors]
            // 2. modify that array
            newColors.push("#FF0000")
            // 3. set it to state
            setColors(newColors)
            // color max
            colorMax(newColors.length)
          }}
        >
          Add color
        </button>
      </div>
      
      <div
        className="gradient-preview"
        style={{
          backgroundImage,
        }}
      />
      
      <div className="colors">
        {colors.map((color, index) => {
          const colorId = `color-${index}`;
          return (
            <div key={colorId} className="color-wrapper">
              <label htmlFor={colorId}>
                Color {index + 1}:
              </label>
              <div className="input-wrapper">
                <input
                  id={colorId}
                  type="color"
                  // add a value to make a controlled
                  value={color}
                  onChange={(element) => {
                      // create a new array
                      const nextColors = [...colors];
                      // mutate it
                      nextColors[index] = event.target.value;
                      // add to state
                      setColors(nextColors);
                      console.log(colors);
                  }}
                />
              </div>
            </div>
          );
        })}
      </div>
    </div>
  );
}

export default App;
```

- Video Solution:
- For the condition AC, to check the minimum and max conditions
- Create a separate function for the `addColor` and `removeColor` with and if statement for the min, max requirement.

```JAVASCRIPT
  function addColor() {
    if (colors.length >=5) {
      window.alert('There is a maximum of 5 colors')
      return; //stops here
    }
     // 1. create a new array
     let newColors = [...colors]
     // 2. modify that array
     newColors.push("#FF0000")
     // 3. set it to state
     setColors(newColors)    
  }

  function removeColor() {
    if(colors.length <=2) {
      window.alert('There is a minimum of 2 colors')
      return; //stops here
    }
      // 1. create a new array
      let removeColor = [...colors]
      // 2. modify that array
      removeColor.pop()
      // 3. set to state
      setColors(removeColor)
  }
```
