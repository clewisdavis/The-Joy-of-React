# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Single Source of Truth

Earlier in the course, we talked about working with forms, we saw how a form input could either be controlled or uncontrolled.

A controlled input is bound to a piece of React state:

```JAVASCRIPT
function App() {
  const [name, setName] = React.useState('');

  return (
    <>
      <label htmlFor="name-input">
        Name:
      </label>
      <input
        id="name-input"
        value={name}
        onChange={(event) => {
          setName(event);
        }}
      />
    </>
  );
}
```

By contrast, an uncontrolled input is free to do its own thing. We might specify an initial value, but we won't control ti with React state:

```JAVASCRIPT
function App() {
  return (
    <>
      <label htmlFor="name-input">
        Name:
      </label>
      <input
        id="name-input"
        defaultValue="Enzo Matrix"
      />
    </>
  );
}
```

When we think about it, what we are really doing here is setting the 'source of truth' for this input:

- Controlled inputs use React state as teh source of truth.
- Uncontrolled inputs use the internal DOM state as the source of truth.

Form controls like `<input>` must have some sort of internal state, managed by the browser, to track what the user typed.

🤔 Realized we can apply this same controlled/uncontrolled idea to the components that we create.

Consider the `Counter` component we saw earlier:

```JAVASCRIPT
function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
}
```

We then render this component without any props, like this:

```JAVASCRIPT
function App() {
  return (
    <Counter />
  );
}
```

From the consumer perspective, this element is totally self-contained. It has its own internal state, and I can't access it. Like an **uncontrolled input**.

Now, let's re-write the code:

```JAVASCRIPT
function App() {
  const [count, setCount] = React.useState(0);

  return (
    <Counter
      count={count}
      onIncrement={() => setCount(count + 1)}
    />
  );
}

function Counter({ count, onIncrement }) {
  return (
    <button onClick={onIncrement}>
      {count}
    </button>
  );
}
```

In this new version, `Counter` is controlled by the consumer. The consumer owns the state, and we are effectively data-binding this `Counter` element to our state variable. Like a controlled text input.

Now, it's not exactly the same, since either way, we are using a React state variable as the source of truth, not the DOM. But from the perspective of the consumer, pretending that `Counter` is a black box, it's remarkably similar.

Which approach is better? Well, it depends on the circumstances
