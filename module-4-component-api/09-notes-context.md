# The Joy of React - Module 4 - Component API Design

- [Course Outline Notes](../course-notes.md)

## Component API Design

### Context

- The main way to pass data inside a React app is through props. Like a train network, props allow us to pass state and other data across the application.

- It can be tedious to pass data through props. To help improve quality of life, React includes a secondary mode of transportation: Context.

- Context is like an express train, it alow us to skip certain stop s and hop straight from one component to another.

#### The Problem

- The problem 'Context' is solving.
- Have a large application with a lot of nested components, and you want to use a piece of data, you have to pass that along through all the components. Called 'prop drilling'.
- But if you have a change in the design, it becomes a nightmare to remember to clean up all the props. You end up with a codebase, with unused props.

![Prop Drilling](images/image-35.png)

- Context solves this problem, similar to Mario, skipping between worlds, context will allow you to jump to any component that is an child.
- Or an express bus stop, going straight from the terminal to the airport.

#### Syntax

- A an example, suppose we have a `favoriteColor` help in state, and we want to make it available to every compoennt in the app. Using context.

- There are two steps to providing context: **providing** and **consuming**.

##### Step 1: Providing

In Step 1, we use a provider to make a particular value available through context. We do this by wrapping our application in a Provider component, which we get from React when creating a context.

- Looks like this:

```JAVASCRIPT
// App.js
import React from 'react';

import Home from './Home';

// Create a new context
export const FavouriteColorContext = React.createContext();

function App() {
  const [
    favouriteColor,
    setFavouriteColor
  ] = React.useState('#EBDEFB');

  // Wrap everything `App` would normally render inside
  // a Provider, and pass our `favouriteColor` state
  // variable as the value:
  return (
    <FavouriteColorContext.Provider value={favouriteColor}>
      <Home />
    </FavouriteColorContext.Provider>
  );
}

export default App;
```

- First, we create a 'context' with the `React.createContext()` method.

- A 'context' can be thought of as a channel, a radio frequency we can use to broadcast data down through the app. 📻 It's the vehicle we use to deliver a value from one spot to another.

- `FavouriteColorContext` is a plain old JS object. It includes a bunch of stuff that react ue internally. It also includes a `Provider` component for us to render.

- When we render `<FavouriteColorContext.Provider>`, we start to broadcast a value, making it available to any descendant component. In this case, we are broadcasting the `favouriteColor` state variable.

- Often, Provider components are kept at the very top of our applications, so that their broadcast can reach anywhere in the app.

##### Step 2: Consuming

- When we want to access this value, we do so like this:

```JAVASCRIPT
import { FavouriteColorContext } from './App';

function Sidebar() {
  const favouriteColor = React.useContext(FavouriteColorContext);
}
```

- `useContext` is a hook designed to 'plug in' to a particular context and pluck out its current value. IN this case, we are handing it the `FavouriteColorContext` object we created in `App`, and it's grabbing the `favouriteColor` value that was funneled through.

- To extend the channel/frequency analogy, `useContext` is like a radio 📻. By passing it the `FavouriteColorContext` value, we are tuning this radio to the correct frequency and `favouriteColor` music starts playing.

- Code Sandbox - [Context](https://codesandbox.io/s/vss206?file=/App.js&utm_medium=sandpack)

##### Updating value in context

- In the example above, the `favoriteColor` state variable is being passed through context as a read-only value. There is no way to change that value.

- What if we wanted to allow some descendant to be able to change the color?

- We can solve for this by **passing the setter function through context as well**. This is similar to the pattern we learned in the Lifting State Up lesson.

- [Updated Sandbox](https://codesandbox.io/s/8tfi1m?file=/App.js&utm_medium=sandpack), the `Sidebar` component now uses both the current state value and teh setter function, to allow the use to change the sidebar's color:

- Pass the `value` as ab object directly in the `Provider`.

```JAVASCRIPT
<PlaybackContext.Provider value={{ playbackRate, setPlaybackRate }}>
  // JSX
</PlaybackContext.Provider>
```

- In your consuming component, don't forget to 'destructure' to consume the values passed in.

```JAVASCRIPT
  function VideoPlayer () {
      // define the context, destructure the values
      const { playbackRate, setPlaybackRate } = React.useContext(PlaybackContext);

      return (
        //JSX
      )
  }

  export default VideoPlayer;
```

- In practice, we almost always pass an object through context, since this allows us to package up multiple values together.

**📣 Here is the basic formula:**

1. Create a new context with `React.createContext`.
2. Use the `Provider` component, from that context, to wrap around the application. Pass it a bundle of values that you ned in other parts of the app.
3. Pluck the data you need from context, with the `useContext` hook.

ℹ️ When to use context:

- When should we use context to pass state around the app, adn when should we use prop-drilling?

- Context is most commonly used for global sate. Global state refers to state that is needed across many different parts of the application, like color theme, or a current logged in user.
- This is in contrast to 'local state', which is used only in a single place within the app.

- 👍 Rule of thumb, keep having to pass prop though a component, probably use context.

```JAVASCRIPT
function AccountSettings({ user }) {
  return (
    <section>
      <UserProfile user={user} />
    </section>
  )
}
```

- The `AccountSettings` component doesn't actually use the `user` prop. It passes it right along to `UserProfile`.
- No right/wrong way, subjective.

#### Exercises, Passing a use object

- Let's update the 'Prop Drilling' sandbox from a couple lessons ago sot hat it uses context.

ACs:

- A new context should be created in App.js, making the `user` state variable available to all other components.
- The `ModuleLessons` component should pluck `user` from context, instead of receiving it through props.
- No need to update `AccountDropdown`, it can continue to receive `user` via props.

- [Solution Sandbox](https://codesandbox.io/s/yt4p7f?file=/ModuleLessons.js&utm_medium=sandpack)

```JAVASCRIPT
// App.js
import React from 'react';

import useUser from './use-user.hook';
import AccountDropdown from './AccountDropdown';
import CourseIndexLayout from './CourseIndexLayout';

// Provide a Context
export const UserContext = React.createContext();

function App() {
  const user = useUser();
  
  // Wrap the context Provider around the App
  return (
    <UserContext.Provider value={user}>
      <AccountDropdown user={user} />
      <CourseIndexLayout />
    </UserContext.Provider>
  );
}

export default App;
```

- Consume the context

```JAVASCRIPT
import React from 'react';

// consume context, import from App
import { UserContext } from './App';

function ModuleLessons() {

  // define context to use below
  const user = React.useContext(UserContext);
  return (
    <>
      User email: {user?.email || 'Not provided'}
    </>
  );
}

export default ModuleLessons;
```

##### Video Playback Rate

- Context is mostly used for 'global' state, things that are necessary all over the application.

- One example might be the 'playback rate' for videos. When a user changes the playback speed for one video, it should change for al videos, no matter where they are in the app!

- Update the sandbox below so that `playbackRate` is stored in context.

ACs:

- The `VideoPlayer` component should receive `playbackRate` through context, rather than through props.
- The `VideoPlayer` component should also receive the setter function, `setPlaybackRate`, through context instead of props.

- [Code Sandbox](https://codesandbox.io/s/pgrn04?file=/App.js&utm_medium=sandpack)

- Step one, define context

```JAVASCRIPT
import React from 'react';

import VideoPlayer from './VideoPlayer';

// set up the context
export const PlaybackContext = React.createContext();

function App() {
  const [playbackRate, setPlaybackRate] = React.useState('1');

  return (
    // Wrap around the App
    // pass in the values as an object
    <PlaybackContext.Provider value={{ playbackRate, setPlaybackRate }}>
      <main>
        <h1>Video Archives</h1>
        
        {DATA.map(({ id, video, createdBy, license }) => (
          <article key={id}>
            <VideoPlayer
              src={video.src}
              caption={video.caption}
              playbackRate={playbackRate}
              setPlaybackRate={setPlaybackRate}
            />
            <dl>
              <dt>Created by</dt>
              <dd>{createdBy}</dd>
              <dt>Licensed under</dt>
              <dd>{license}</dd>
            </dl>
          </article>
        ))}
      </main>
    </PlaybackContext.Provider>
  );
}

const DATA = [
  {
    id: 'snowstorm',
    video: {
      src: 'https://sandpack-bundler.vercel.app/videos/snowstorm.mp4',
      caption: 'A peaceful snowstorm in a residential area',
    },
    createdBy: 'Karolina Grabowska',
    license: 'Creative Commons Zero (CC0)',
  },
  {
    id: 'flowers',
    video: {
      src: 'https://sandpack-bundler.vercel.app/videos/flowers.mp4',
      caption: 'Macro video of a flower blowing in the wind',
    },
    createdBy: 'Imam Hossain',
    license: 'Creative Commons Zero (CC0)',
  },
  {
    id: 'plane',
    video: {
      src: 'https://sandpack-bundler.vercel.app/videos/plane.mp4',
      caption: 'Plane flying over the clouds',
    },
    createdBy: 'Ahmet Akpolat',
    license: 'Creative Commons Zero (CC0)',
  },
];

export default App;
```

- Step two, consume the context provider

```JAVASCRIPT
import React from 'react';

// consume the provider
import { PlaybackContext } from './App';

function VideoPlayer({
  src,
  caption,
}) {
  // Define the context
  // Object Destructure the values
  const { playbackRate, setPlaybackRate } = React.useContext(PlaybackContext);
  
  const playbackRateSelectId = React.useId();

  const videoRef = React.useRef();

  React.useEffect(() => {
    videoRef.current.playbackRate = playbackRate;
  }, [playbackRate]);

  return (
    <div className="video-player">
      <figure>
        <video ref={videoRef} controls src={src} />
        <figcaption>{caption}</figcaption>
      </figure>

      <div className="actions">
        <label htmlFor={playbackRateSelectId}>
          Select playback speed:
        </label>
        <select
          id={playbackRateSelectId}
          value={playbackRate}
          onChange={(event) => {
            setPlaybackRate(event.target.value);
          }}
        >
          <option value="0.5">0.5</option>
          <option value="1">1</option>
          <option value="1.25">1.25</option>
          <option value="1.5">1.5</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>
      </div>
    </div>
  );
}

export default VideoPlayer;
```

#### Provider Components

- Should you have one context for everything or separate context for every item?
- Best practice to have separate context for each piece of functionality.

```JAVASCRIPT
// App.js
export const UserContext = React.createContext();
export const ThemeContext = React.createContext();
export const PlaybackRateContext = React.createContext();

function App() {
  const [user, setUser] = React.useState(null);
  const [theme, setTheme] = React.useState('light');
  const [playbackRate, setPlaybackRate] = React.useState(1);

  return (
    <UserContext.Provider value={user}>
      <ThemeContext.Provider value={theme}>
        <PlaybackRateContext.Provider value={playbackRate}>
          <Homepage />
        </PlaybackRateContext.Provider>
      </ThemeContext.Provider>
    </UserContext.Provider>
  );
}
```

- Can't you combine all this into one context?

```JAVASCRIPT
export const AppContext = React.createContext();
```

- Not a good practice? Why?

- For performance, you don't want to cause unnecessary re-renders.
- And code readability.

- What about all the logic for each provider? Your `App.js` can get really cluttered up with logic for each provider.
- Recommended you use a **provider component pattern**.
- This will manage everything for each provider, and make for better code readability.

- 🤔 Here is the `App.js` using this pattern. All of the logic for each provider is included within the 'provider component'.

```JAVASCRIPT
//App.js
import Homepage from './Homepage';
import UserProvider from './UserProvider';
import ThemeProvider from './ThemeProvider';
import PlaybackRateProvider from './PlaybackRateProvider';

function App() {
  return (
    <UserProvider>
      <ThemeProvider>
        <PlaybackRateProvider>
          <Homepage />
        </PlaybackRateProvider>
      </ThemeProvider>
    </UserProvider>
  );
}
```

##### Practice - extracting two more provider components

The sandbox picks up from the above. The `UserProvider` component has been created and it' sup to you to create two new provider components: `ThemeProvider` and `PlaybackRateProvider`.

ACs:

- The `ThemeProvider` component should manage everything related to the `theme` state and context.
- The `PlaybackRateProvider` component should manage everything related to the `playbackRate` state and context.
- `App.js` should import and use the two new components, matching the style/format of the `UserProvider` component.
- Inside `Homepage.js` the imports should be updated so that we are importing the contexts from the provider components, not the `App`.

- [Code Playground](https://codesandbox.io/s/d5p7w2?file=/App.js&utm_medium=sandpack)

- Meh, very confusing, which is the context and which is the provider. 🤔 Probably need more practice with this one.
- Checkout the solution code here, [Provider Component Exercise](https://codesandbox.io/s/kosxl6?file=/App.js&utm_medium=sandpack).

##### Performance

In the last module, we learned hwo to crete 'pure' components with React.memo. A pure component is one that doesn't re-render unless its props or state changes.

- 🤔 But what happens when a pure component consumes a context? For example;

```JAVASCRIPT
import { FavouriteColorContext } from './App';

function Sidebar() {
  const favouriteColor = React.useContext(FavouriteColorContext);

  return (
    <div style={{ backgroundColor: favouriteColor }}>
      Sidebar
    </div>
  )
}

export default React.memo(Sidebar);
```

- By wrapping `Sidebar` with `React.memo`, we produce a pure component, but what effect does that have? WHen will this component re-render?

- You can think of context as 'internal props'. It follows all the same rules as props. If the value held in context changes, this component will re-render.

- It is functionally equivalent to this:

```JAVASCRIPT
function Sidebar({ favouriteColor }) {
  return (
    <div style={{ backgroundColor: favouriteColor }}>
      Sidebar
    </div>
  )
}

export default React.memo(Sidebar);
```

So, our updated definition for how pure components work:

```CODE
A pure component will re-render if a prop, state variable, or context value changes.
```

##### Memoizing context values

In most situations we won't be passing a single value through context. We pass several things, packaged up in an object. For example;

```JAVASCRIPT
// App.js
export const FavouriteColorContext = React.createContext();

function App() {
  const [
    favouriteColor,
    setFavouriteColor
  ] = React.useState('#EBDEFB');

  return (
    <FavouriteColorContext.Provider
      value={{ favouriteColor, setFavouriteColor }}
    >
      <Home />
    </FavouriteColorContext.Provider>
  );
}
```

- We are passing the state value, `favouriteColor`, as well as the state-setter function, `setFavouriteColor`.
- When we pass multiple values like this, it tends to wreak havoc on pure components.

- Let's solve this, the playground has a `ColorPicker` component that consumes the `FavouriteColor` context.
- There is also an unrelated piece of state, `count`.

- `ColorPicker` is a pure component, and it doesn't depend at all on the `count` variable, but it re-renders when a `count` changes.
- Your mission is to figure out why this happens, and to fix it, sot hat `ColorPicker` only re-renders when the `favouriteColor` state changes.

ACs:

- Clicking the 'Count: 0' button should NOT cause the `ColorPicker` component to re-render.
- A 'ColorPicker rendered' message is logged whenever `ColorPicker` re-renders, and so you will know you have succeeded once clicking the 'Count' button doesn't spawn a console message.

- 🤔 What is going on here?
- Have to remember, you can only pass a single value through context, `value={{ favouriteColor, setFavouriteColor }}`, we are not passing two different values, we are passing a single object, that holds multiple values. An object with two key value pairs.

- To prove the point, you can pull it out and make a single value, and pass it along to your `Provider`.
- 🤔 The problem is, the `value` object is being regenerated on every single render. Triggering a re-render.
- Whenever something changes in the object `value={{ favouriteColor, setFavouriteColor }}`, it creates a new object, which triggers a re-render.

```JAVASCRIPT
import React from 'react';

export const FavouriteColorContext = React.createContext();

function FavouriteColorProvider({ children }) {
  const [
    favouriteColor,
    setFavouriteColor
  ] = React.useState('#EBDEFB');

  // make it a single value to pass to your provider
  const value = { favouriteColor, setFavouriteColor };
  
  return (
    <FavouriteColorContext.Provider
      value={value}
    >
      {children}
    </FavouriteColorContext.Provider>
  );
}

export default FavouriteColorProvider;
```

- How to solve this?
- We store the value in a `useMemo` and define the dependency so that it only triggers a re-render when the state variable is changed.

```JAVASCRIPT
// FavouriteColorProvider.js
import React from 'react';

export const FavouriteColorContext = React.createContext();

function FavouriteColorProvider({ children }) {
  const [
    favouriteColor,
    setFavouriteColor
  ] = React.useState('#EBDEFB');

  // Store the value, in a useMemo
  // add the dependency favouriteColor to trigger a re-render
  const value = React.useMemo(() => {
    return { favouriteColor, setFavouriteColor };
  }, [favouriteColor])
  
  return (
    <FavouriteColorContext.Provider
      value={value}
    >
      {children}
    </FavouriteColorContext.Provider>
  );
}

export default FavouriteColorProvider;
```

- 🤔 This hurts my brain, but a shortcut to remember, as you are working with a `.Provider`.
- Anytime you pass an object to an context provider, `<FavouriteColorContext.Provider value={value}>`, which is most of the time.
- 📣 You want to 'memoize' that object, so that we only re-render when something inside the object changes.

```JAVASCRIPT
  const value = React.useMemo(() => {
    return { favouriteColor, setFavouriteColor };
  }, [favouriteColor])
```
