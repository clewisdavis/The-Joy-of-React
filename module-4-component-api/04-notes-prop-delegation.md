# The Joy of React - Module 4 - Component API Design

- [Course Outline Notes](../course-notes.md)

## Component API Design

## Prop Delegation

In the `Banner` example from the Spectrum lesson, we saw how our `LoggedIn` banner had to "forward" some props along:

```JAVASCRIPT
function LoggedInBanner({
  user,
  // These two props:
  type,
  children,
}) {
  if (
    !user ||
    user.registrationStatus === 'unverified'
  ) {
    return null;
  }

  // ...are forwarded along to Banner:
  return <Banner type={type}>{children}</Banner>;
}
```

- 🤔 What if the component had 10 forwarded props instead of 2? We would have to list them all out, one by one?
- React has a solution for that, instead we can take advantage of rest parameters and spread syntax
  - [JS Primer: Rest and Spread](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/12-rest-spread)

- Here is what it looks like:

```JAVASCRIPT
function LoggedInBanner({
  user,
  // Collect all unspecified props:
  ...delegated
}) {
  if (
    !user ||
    user.registrationStatus === 'unverified'
  ) {
    return null;
  }

  // And pass them onto Banner:
  return <Banner {...delegated} />
}
```

- Chosen the name `delegated` ebcause it feels symantically appropriate, but we can name this variable whatever we want, some prefer `rest`:

- If you `console.log` the `delegated` variable, we see an object containing all of the other props provided to this component. For example:

```JAVASCRIPT
console.log(delegated);
/*
  {
    type: 'success',
    children: 'Account registered!',
  }
*/
```

- To apply these props onto our `Banner` element, we create an expression slot with curly brackets, and spread the props along using the spread syntax `...`:

```JAVASCRIPT
// This code: 
<Banner {...delegated} />

// ...is equivalent to this code: 
<Banner
    type={delegated.type}
    children={delegated.children}
/>

// ...which is the same thing as this code:
<Banner type={delegated.type}>
  {delegated.children}
</Banner>
```

- To go one step further in demystifying this new syntax, here's hwo it gets transpiled to plain JS:

```JAVASCRIPT
// This JSX...
<UserProfileCard user={currentUser} {...delegated} />

// ...turns into this JavaScript:
React.createElement(
  UserProfileCard,
  {
    user: currentUser,
    ...delegated
  }
);
```

- 🛑 One gatcha, the `...delegated` item needs to be the final item in the list. And cannot have a trailing comma, you will get an error.

```JAVASCRIPT
function Slider({
  label,
  ...delegated, // <-- This comma is a problem
}) {}
```

### Supercharged HTML tags

- Imagine you have a component, that is a text input.

```JAVASCRIPT
import React from 'react';

function TextInput({ 
    id, 
    label, 
    type,
    value,
    onChange,
}) {
  const generatedId = React.useId();
  const appliedId = id || generatedId;
  
  return (
    <div className="text-input">
      <label htmlFor={appliedId}>
        {label}
      </label>
      <input
        id={appliedId}
        type={type}
        value={value}
        onChange={onChange}
      />
    </div>
  );
}

export default TextInput;
```

- You only have access to the things you exposed via props.
- You cannot provide, whatever attribute you would normally put on a text input, `required={true}` or a `minLength={12}` for example.
- React will not recognize those attributes if not provided via props. React has no idea that a component is supposed to be a wrapper around your `input` field
- All React knows, is we created a component, and it returns some JSX.
- And given them `props`, to work with.

- 📣 If you want your component `TextInput`, to work with attributes of the `input`, you have to use the prop delegation trick.

- If you have a bunch of props, you can combine them into a `...delegated` object

```JAVASCRIPT
function TextInput({ id, label, ...delegated }) {
    return (
        // JSX here...
    )
}
```

- Then in your JSX, spread the delegated object onto your element, this case the `input` field.
- 🆒 You can choose where to put your `...delegated` props, can go on any element, and be passed in.

```JAVASCRIPT
// TextInput.js
import React from 'react';

function TextInput({ id, label, ...delegated }) {
  const generatedId = React.useId();
  const appliedId = id || generatedId;
  
  return (
    <div className="text-input">
      <label htmlFor={appliedId}>
        {label}
      </label>
      <input
        id={appliedId}
        // spread the ...delegated props
        {...delegated}
      />
    </div>
  );
}

export default TextInput;
```

- When you use the component, you can add a bunch of props, and passing into the component.
- All of the stuff you specify in the `input` is automatically being delegated along.

```JAVASCRIPT
    <TextInput
      required={true}
      data-test-id="login-email-field"
      label="Email"
      type="email"
      value={email}
      onChange={(event) => {
        setEmail(event.target.value);
      }}
    />
```

- Full component: [LogIn Form, Delegated](https://codesandbox.io/s/ckz6pc?file=/LoginForm.js&utm_medium=sandpack)

```JAVASCRIPT
// LogIn form...adding a bunch of properties to the TextInput
import React from 'react';

import TextInput from './TextInput';

function LoginForm() {
  const [email, setEmail] = React.useState('');
  const [password, setPassword] = React.useState('');

  function handleLogin() {
    alert(`Logged in with ${email}`);
  }

  return (
    <form onSubmit={handleLogin}>
      <TextInput
        required={true}
        data-test-id="login-email-field"
        label="Email"
        type="email"
        value={email}
        onChange={(event) => {
          setEmail(event.target.value);
        }}
      />
      <TextInput
        required={true}
        minLength={12}
        label="Password"
        type="password"
        value={password}
        onChange={(event) => {
          setPassword(event.target.value);
        }}
      />
      <button>
        Submit
      </button>
    </form>
  );
}

export default LoginForm;
```

### Exercises, Prop Delegation

Exercises for Prop Delegation

#### A Slider Component

- In the [sandbox](https://codesandbox.io/s/f6m2ku?file=/Slider.js&utm_medium=sandpack), you have a `Slider` component.

- Internally, this component renders an `<input type"range">` the built-in DOM neod for creating ranges and sliders. And so we can think of thi s`Slider` component as a "supercharged" range input.

- Unfortunately, we've only exposed a subset of attributes we might wish to set on this DOM node through props.
- Your mission is to forward all props on to the input, so that we can treat `<Slider>` as a supercharged range input.

**ACs**

- The `Slider` component should use the prop delegation technique to forward all unspecified props to the `<input type="range">` that renders.

- Add the `...delegated` rest parameter to your component as a prop.
- And then spread that onto your element in the JSX, the `<input>` slider.

```JAVASCRIPT
import React from 'react';

import styles from './Slider.module.css';

// add the ...delegated rest parameter
function Slider({ label, ...delegated }) {
  const id = React.useId();
  
  return (
    <div className={styles.wrapper}>
      <label
        htmlFor={id}
        className={styles.label}
      >
        {label}
      </label>
      <input
        type="range"
        id={id}
        className={styles.slider}
        // spread that onto the input
        {...delegated}
      />
    </div>
  );
}

export default Slider;
```

#### A Toggle Component

- Toggle components are similar to checkboxes. They are often used to flip a value on or off.
- We have a custom `Toggle` component, like this:

![Toggle](images/image-29.png)

- We want to be abe to pass a custom `className` to provide custom styles. For example, passing a CSS class that updates the color of the toggle circle.

![Toggle Circle](images/image-30.png)

- Your mission is to update the `Toggle` component so that it can be customized, by passing a `className`. It should also support additional custom props, like data attributes.

**ACs**

- In the example below, the second `<Toggle>` instance has a prop: `className="green-toggle`. This class should be applied to the `<button>` inside the `Toggle` component, producing the green circle shown in the GIF above.
- Other props (eg. data attributes) should also be forwarded along to the `<button>` element.

Note: It may be helpful to review the [Conflicts section](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/12-rest-spread) of the Rest/Spread primer lesson.

- [Code Sandbox Toggle](https://codesandbox.io/s/0t2xol?file=/App.js&utm_medium=sandpack)

- Want to have the built in CSS class `toggle`, as well as the modifier CSS class `green-toggle`.
- The way to do this, is to make a prop for `className`, and to manually add the class in our component.
- Create an expression slot, and use a template string, and then interpolate in the CSS `className`

```JAVASCRIPT
// Toggle.js
function Toggle({
    label,
    checked,
    onClick,
    className,
    ...delegated
}) {
    return (
      <button
        id={id}
        // template string to interpolate the supplied className
        className={`toggle ${className}`}
        type="button"
        aria-pressed={checked}
        onClick={onClick}
        {...delegated}
      >
    )
}
```

- Small bug 🐛: When you interpolate a prop, without providing a value, React will translate to the string `undefined`. And you end up with a CSS `className` of `undefined`, which is kind of strange.

![Undefined](images/image-31.png)

- To fix this, just set a default value of `''`, on the `className` prop, which resolve to nothing.

```JAVASCRIPT
function Toggle({
  label,
  checked,
  onClick,
  className = '',
  ...delegated
}) {
    // code here
}
```

- Full solution: Toggle Component

```JAVASCRIPT
// Toggle.js
import React from 'react';

function Toggle({
  label,
  checked,
  onClick,
  className = '',
  ...delegated
}) {
  const id = React.useId();
  
  // This style updates the UI, to move the ball
  // and indicate whether it's toggled or not.
  const ballStyle = {
    transform: checked
      ? `translateX(100%)`
      : `translateX(0%)`,
  };
  
  return (
    <div className="wrapper">
      <label
        htmlFor={id}
        className="label"
      >
        {label}
      </label>
      <button
        id={id}
        className={`toggle ${className}`}
        type="button"
        aria-pressed={checked}
        onClick={onClick}
        {...delegated}
      >
        <span className="ball" style={ballStyle} />
      </button>
    </div>
  );
}

export default Toggle;
```

```JAVASCRIPT
// App.js
import React from 'react';

import useToggle from './hooks/use-toggle';
import Toggle from './Toggle';

function App() {
  const [enableWifi, toggleEnableWifi] = useToggle(true);
  const [lowPowerMode, toggleLowPowerMode] = useToggle(false);
  
  return (
    <main>
      <Toggle
        label="Enable Wi-Fi"
        checked={enableWifi}
        onClick={toggleEnableWifi}
      />
      <Toggle
        className="green-toggle"
        label="Low Power Mode"
        checked={lowPowerMode}
        onClick={toggleLowPowerMode}
      />
    </main>
  );
}

export default App;
```

#### Conflicts

In the "Toggle" exercise from previous lesson, we saw how delegated props can lead to some issues if there are conflicts.

- For example:

```JAVASCRIPT
function Checkbox({ label, ...delegated}) {
  const id = React.useId();

  return (
    <>
      <label htmlFor={id}>
        {label}
      </label>
      <input
        id={id}
        type="checkbox"
        {...delegated}
      />
    </>
  );
}
```

- This `Checkbox` component applies two hardcoded attributes to the `<input>`, `type` and `id`.
- Now suppose the consumer of this component uses it like this:

```JAVASCRIPT
<Checkbox 
  label="Do you agree with terms?"
  type="button"
  onClick={handleAgreeTerms}
/>
```

- The `type` and `onClick` props are not specified in the `Checkbox` component, and so thy are collected into the `delegated` object, and pasted onto the `<input>`:

```JAVASCRIPT
// Here's the React element that will be created:
<input
  id={id}
  type="checkbox"
  type="button"
  onClick={handleAgreeToTerms}
/>
```

- We have specified two different values for `type` and when there are conflicts like this, **later values overwrite earlier ones**. And this input will be a button instead of a checkbox.

📣 Essentially the consumer has "hacked" our Checkbox component to not render a checkbox!

- Instead, re-write our `Checkbox` component to spread the provided props first:

```JAVASCRIPT
function Checkbox({ label, ...delegated}) {
  const id = React.useId();

  return (
    <>
      <label htmlFor={id}>
        {label}
      </label>
      <input
        {...delegated}
        id={id}
        type="checkbox"
      />
    </>
  );
}
```

- With this change, the same `<Checkbox>` element produces a different result:

```JAVASCRIPT
<input
  // Delegated props:
  type="button"
  onClick={handleAgreeToTerms}
  // Built-in attributes:
  id={id}
  type="checkbox"
/>

// After removing the duplicate `type`, we're left with:
<input
  onClick={handleAgreeToTerms}
  id={id}
  type="checkbox"
/>
```

- 📣 Because we flipped the order, the user-supplied `type="button"` will now be overwritten by the built-in `type="checkbox`.

#### A powerful tool in API design

- When we produce React components, we get to decide how much power we want to give consumers. We can choose which properties they are allowed to overwrite, adn which ones are mandatory / locked in.

- In the example above, the `Checkbox` should always render an `<input type="checkbox">`, and so I don't want to let consumers overwrite the `type` attribute.

- But this is not always the case. Sometimes, you want the consumer to overwrite the built-in attributes.

```JAVASCRIPT
function ArrowIcon({ size, ...delegated }) {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
      width={size}
      height={size}
    >
      <path
        d="M 20 0 L 24 12 L 0 12 L 24 12 L 20 24"
        stroke="black"
        strokeLinecap="round"
        {...delegated}
      />
    </svg>
  );
}
```

- By default, this component will render a black arrow with rounded lines, but I can supply my own overrides:

```JAVASCRIPT
<ArrowIcon stroke="red" strokeLinecap="square" />
```

- No right/wrong answer when it comes to where the `{...delegated}` should go.
- It is a choice we can use as a tool, to decide how much power/flexibility I want to grant to the developers consuming the component.

#### Manually managing conflicts

Sometimes, delegated props is to blunt of a tool, we need to do some manual work to resolve the conflict.

- For example, when it comes to CSS classes, we often want to apply both the user-supplied class as well as the built-in one.

- In teh Toggle exercise, we manually merged teh two classes together sot hat we were applyign the `toggle` class as well as the `green-toggle` class.

- I've built a lot of components that follow this exact template. Here is the bare bones example:

```JAVASCRIPT
function Template({ className = '' }) {
  const appliedClass = `built-in-class ${className}`;

  return (
    <div
      className={appliedClass}
    />
  );
}
```

- In a sense, we seen this example of this pattern already, when we talked about the Rules of Hooks:

```JAVASCRIPT
function TextInput({ id, label, type }) {
  let generatedId = React.useId();
  let appliedId = id || generatedId;

  return (
    <div className="text-input">
      <label htmlFor={appliedId}>
        {label}
      </label>
      <input
        id={appliedId}
        type={type}
      />
    </div>
  );
}
```

- If the user supplies an `id` prop, it will be used for the input's `id`, and the label's `htmlFor`.
- If they don't , we will use the generated value we get from the `React.useId` hook.

- We could rely on rest/spread to apply the correct `id` on the `<input>`, but we also need to set the exact same value on the `<label>`, via the `htmlFor` attribute. As a result we need to manage ths conflict manually.

- Here is one more example, where we can supply custom inline styles to a component that already has some.

```JAVASCRIPT
function ExampleComponent({
  // User-specified styles.
  // Defaults to an empty object so that we always receive an
  // object, never “undefined”:
  style = {},
  children,
  ...delegated
}) {
  const builtInStyle = {
    padding: 16,
    background: 'red',
  };

  return (
    <div
      {...delegated}
      style={{
        // Merge both sets of styles, prioritizing the
        // built-in styles:
        ...style,
        ...builtInStyle,
      }}
    >
      {children}
    </div>
  );
}
```

- To review, we have several options when it comes to conflicting attributes.

1. If we want the consumer to overwrite a particular hardcoded attribute, we can place the `{...delegated}` syntax afterwards.
2. If we want to prioritize the hardcoded attribute, however, the `{...delegated}` syntax should come first.
3. If we want to merge both values, we will need to manage it ourselves, without using `{...delegated}`.

All 3 of these options are valid in different situations. 📣 It all comes down to how much control we want to grant the consumer.

#### Delegating Styles

- How do you manage low level component, and allow them to be used and styled in many differ situations.
- Allow for flexibility when it comes to styles.
- A low level component `<Slider/>`, on the spectrum, might be used in many places in a larger React application.
- 📣 Lots of different context, lots of different designs, we want to be flexible enough, to adapt to all those circumstances.

- In the example; building a `VolumeSlider` component that consumes the lower level `<Slider />` component, and we want to make a couple tweaks
  - Update the handle size
  - And the color of the handle

```JAVASCRIPT
import React from 'react';

import Slider from './Slider_props';
import styles from './VolumeSlider.module.css';

function VolumeSlider({ volume, setVolume }) {
  return (
    <Slider
      label="Volume"
      min={0}
      max={100}
      value={volume}
      onChange={(event) => {
        setVolume(event.target.value);
      }}
    />
  );
}

export default VolumeSlider;
```

- 🤔 How do you allow the lower level `<Slider />` component to be customized? Two main ways:

1. Add explicit `props` for all the things, we want to customize. For example, add the `handleSize`, `handleColor` and `handleActiveColor` props to the low level `Slider` component

```JAVASCRIPT
function VolumeSlider({ volume, setVolume }) {
  return (
    <Slider
      label="Volume"
      min={0}
      max={100}
      value={volume}
      // explicitly customize
      handleSize={12}
      handleColor="green"
      handleActiveColor="lightGreen"
      //
      onChange={(event) => {
        setVolume(event.target.value);
      }}
    />
  );
}
```

2. Add a `className = ''` prop to the low level component, and the consumer can supply whatever `className` they want. Then create a variable, that will combine the consumer supplied `className` with the baseline styles.

```JAVASCRIPT
import React from 'react';

import styles from './Slider.module.css';

function Slider({ label, className = '', ...delegated }) {
  const id = React.useId();
  
  // Combine the baseline styles, with the consumer supplied
  const sliderClassName = `${styles.slider} ${className}`;

  return (
    <div className={styles.wrapper}>
      <label htmlFor={id} className={styles.label}>
        {label}
      </label>
      <input
        {...delegated}
        type="range"
        id={id}
        // Pass all styles in
        className={sliderClassName}
      />
    </div>
  );
}

export default Slider;
```

- And instead of setting all the `props` individually like above, you just set `className={styles.volumeSlider}` on the low level component. And the styles, will be within your CSS module `VolumeSlider.module.css`.

```JAVASCRIPT
// VolumeSlider
import React from 'react';

import Slider from './Slider_className';
// Add your styles
import styles from './VolumeSlider.module.css';

function VolumeSlider({ volume, setVolume }) {
  return (
    <Slider
      label="Volume"
      min={0}
      max={100}
      value={volume}
      // Pass styles into the component
      className={styles.volumeSlider}
      onChange={(event) => {
        setVolume(event.target.value);
      }}
    />
  );
}

export default VolumeSlider;
```

- Any modification you want to make, just add to your CSS module

```CSS
.volumeSlider {
  --handle-size: 12px;
  --handle-color: red;
  --handle-active-color: lightgreen;
}
```

- 📣 Pass a `className` and declare whatever tweaks we want in the CSS file.
- 📣 Or, create a limited number of props, and use those props within the body of the component.

- 🤔 Which of these approaches do you prefer, and Pros and Cons?

- Big debate with no consensus among the React community. The biggest difference here is how much power you want to grant the consumer of your component.

- With the `className` approach, developers can apply any CSS they want to the `<input>`.
- For example; we could add some CSS to make the slider `vertical`.

```CSS
.verticalSlider {
  width: 100px;
  transform: rotate(-90deg);
  margin: 50px;
}
```

- 📣 By exposing the `className` prop, we grant teh consumer a lot of control. With individual props, we limit that control.

#### The arguments for specific props

- When building low level components, likely to implement them according to a design system.

- A design system is a document that provides guidelines and rules on using each component. The components we implement should only allow the customizations specified in the design system.

- In other words, we want to make sure that developers "color within the lines". By exposing a `className` prop, a rogue developer could radically change how this component is styled, in violation of the design system.

- Components are meant to encapsulate markup, logic and styles. What is the point of having a DS if the developer can do whatever they want with the aesthetic?

- We can always update the DS as requirements change. But it's chaos to allow developers to apply any CSS they like to these components.

#### The argument for "className" prop

- In a real world app, the 'specific props' approach becomes unwieldy.
- What if wanted to speficy a hover color? Customize the handle size, but only for a specific media query?

- CSS is a huge and sprawling language. We could end up with 50+ props, which would be a nightmare to maintain.
- What if a designer comes up with a legitimate use case that doesn't work with our specific props?

- Here is what will happen, developers will reach around React altogether, and apply whatever CSS they want.

```CSS
/* HACK: Apply rotation to Slider component */
.some-wrapper form input[type="range"] {
  margin: 50px !important;
  transform: rotate(90deg) !important;
}
```

- Cannot stop a developer from applying CSS to a particular element. A determined developer can still apply whatever styles they want.
- Or worse, they decide to create an `SliderAlt` that is 95% the same, but different in this one regard. Some codebase have multiple near-identical components because the prop interface wasn't flexible enough.

- 📣 It is just not realistic to come up a handful of style-related props that work for every possible use case. The world is to messy for that.

### Final thought

- Go with the `className` approach.
