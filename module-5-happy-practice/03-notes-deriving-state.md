# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Deriving State

- Best practice, store the minimum amount of information in state. And if something can be derived from state, it should be.

### Exercise, Compose a Tweet

In the older versions of Twitter, a number showed how many characters you had left to use in your message

- In the playground below, we have recreated the implementation but it's not as nice. Improve the code to improve it.

ACs:

- The number of characters remaining should be derived from the `message` sate, not stored in a state variable.
- No effects should be used.

- Notes:

- Simplify the implementation by removing the `useEffect` and using the `message` state directly.
- Also remove the other state `charsRemaining` and `setCharsRemaining` and instead, use the `message` state.

```JAVASCRIPT
function ComposeTweet({ maxChars, handleSubmit }) {
  
  const [message, setMessage] = React.useState('');

  // replace the effect, with simple logic derived from state
  // to get the remaining characters
  const charsRemaining = maxChars - message.length;

  // remove this state, you can derive what you need from the message state
  // const [charsRemaining, setCharsRemaining] =
  //   React.useState(maxChars);

  const id = React.useId();

  // remove the Effect, not necessary
  // React.useEffect(() => {
  //   setCharsRemaining(maxChars - message.length); 
  // }, [maxChars, message]);

  return (
    //JSX Here
  )
}

export default ComposeTweet;
```

- Why not use an Effect?

- Every time the user puts a character in the text field, it triggers a re-render of the component.
- The `message` state has been updated, but the other state, `charsRemaining` is not updated.
- It's only after React does of the work of re-render, and updating the DOM, that we call another state change using the `useEffect`
- Then that goes through the process of re-creating all the elements again.

- Compared to the solution:
- Calculating the characters remaining happens in the little bit of logic `maxChars - message.length` and we only have to render the component once.
- And we only need to do a single render, to update both the parts of the UI, the text area and the characters remaining.

- Sandbox - [Deriving State - Chars Remaining](https://codesandbox.io/p/sandbox/compassionate-hooks-flyg5y?layout=%257B%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522rootPanelGroup%2522%253A%257B%2522direction%2522%253A%2522horizontal%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522id%2522%253A%2522ROOT_LAYOUT%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522clqayeere00063b5vxpzsxe0r%2522%252C%2522sizes%2522%253A%255B70%252C30%255D%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522EDITOR%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522id%2522%253A%2522clqayeere00023b5vfm35ida2%2522%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522SHELLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522id%2522%253A%2522clqayeere00033b5v356blija%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522DEVTOOLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522id%2522%253A%2522clqayeere00053b5v576khxg5%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%252C%2522sizes%2522%253A%255B50%252C50%255D%257D%252C%2522tabbedPanels%2522%253A%257B%2522clqayeere00023b5vfm35ida2%2522%253A%257B%2522id%2522%253A%2522clqayeere00023b5vfm35ida2%2522%252C%2522tabs%2522%253A%255B%255D%257D%252C%2522clqayeere00053b5v576khxg5%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clqayeere00043b5vp1imzrp5%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522UNASSIGNED_PORT%2522%252C%2522port%2522%253A0%252C%2522path%2522%253A%2522%252F%2522%257D%255D%252C%2522id%2522%253A%2522clqayeere00053b5v576khxg5%2522%252C%2522activeTabId%2522%253A%2522clqayeere00043b5vp1imzrp5%2522%257D%252C%2522clqayeere00033b5v356blija%2522%253A%257B%2522tabs%2522%253A%255B%255D%252C%2522id%2522%253A%2522clqayeere00033b5v356blija%2522%257D%257D%252C%2522showDevtools%2522%253Atrue%252C%2522showShells%2522%253Atrue%252C%2522showSidebar%2522%253Atrue%252C%2522sidebarPanelSize%2522%253A15%257D)

### Exercise, Checkout Flow
