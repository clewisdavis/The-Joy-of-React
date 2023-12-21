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

In this exercise, working on a checkout flow, the 'shopping cart' we worked on back in module 1, now includes info about the price.

But there is a bug: removing an item from the cart doesn't update the subtotal/tax/total values.

ACs:

- The numbers at the bottom of the page (Subtotal / Taxes / Total) should recalculate when removing an item from the cart.
- You should remove any state variable that are not necessary. Derive state whenever possible.

- Solution Notes:
- The `CheckoutFlow` component has a lot of unecessary state variables declared.
- You can simplify this, by just creating a simple variable for each and adding the JS logic.

```JAVASCRIPT
function CheckoutFlow({ items, taxRate, handleDeleteItem }) {

  //Remove all these state variables
  // const [subtotal, setSubtotal] = React.useState(() => calculateSubtotal(items));
  // console.log(subtotal);
  // const [taxes, setTaxes] = React.useState(subtotal * taxRate);
  // const [total, setTotal] = React.useState(subtotal + taxes);

  // Add them as JS variables, these will update when the component re-renders.
  const subtotal = calculateSubtotal(items);
  const taxes = subtotal * taxRate;
  const total = subtotal + taxes;

  
  return (
    <>
      <h1>Checkout</h1>
      
      <CartTable items={items} handleDeleteItem={handleDeleteItem} />
      
      <table className="checkout-totals">
        <tbody>
          <tr>
            <th scope="col">Subtotal</th>
            <td>${roundTo(subtotal, 2)}</td>
          </tr>
          <tr>
            <th scope="col">Taxes</th>
            <td>${roundTo(taxes, 2)}</td>
          </tr>
          <tr>
            <th scope="col">Total</th>
            <td>${roundTo(total, 2)}</td>
          </tr>
        </tbody>
      </table>
    </>
  )
}
```

- See full code sandbox - [Checkout](https://codesandbox.io/p/sandbox/checkout-derive-state-njhwjy?file=%2FCheckoutFlow.js%3A16%2C34&layout=%257B%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522rootPanelGroup%2522%253A%257B%2522direction%2522%253A%2522horizontal%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522id%2522%253A%2522ROOT_LAYOUT%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522clqcd4aok00063b5wntxnx4qj%2522%252C%2522sizes%2522%253A%255B100%252C0%255D%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522EDITOR%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522id%2522%253A%2522clqcd4aok00023b5w4ecknt2h%2522%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522SHELLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522id%2522%253A%2522clqcd4aok00033b5ws2bmdp62%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522DEVTOOLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522id%2522%253A%2522clqcd4aok00053b5wctfivecs%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%252C%2522sizes%2522%253A%255B58.10352222111929%252C41.89647777888071%255D%257D%252C%2522tabbedPanels%2522%253A%257B%2522clqcd4aok00023b5w4ecknt2h%2522%253A%257B%2522id%2522%253A%2522clqcd4aok00023b5w4ecknt2h%2522%252C%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clqcdpc9700023b5whfb6wovn%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522FILE%2522%252C%2522initialSelections%2522%253A%255B%257B%2522startLineNumber%2522%253A16%252C%2522startColumn%2522%253A34%252C%2522endLineNumber%2522%253A16%252C%2522endColumn%2522%253A34%257D%255D%252C%2522filepath%2522%253A%2522%252FCheckoutFlow.js%2522%252C%2522state%2522%253A%2522IDLE%2522%257D%255D%252C%2522activeTabId%2522%253A%2522clqcdpc9700023b5whfb6wovn%2522%257D%252C%2522clqcd4aok00053b5wctfivecs%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clqcd4aok00043b5wzk9o4k5r%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522UNASSIGNED_PORT%2522%252C%2522port%2522%253A0%252C%2522path%2522%253A%2522%252F%2522%257D%255D%252C%2522id%2522%253A%2522clqcd4aok00053b5wctfivecs%2522%252C%2522activeTabId%2522%253A%2522clqcd4aok00043b5wzk9o4k5r%2522%257D%252C%2522clqcd4aok00033b5ws2bmdp62%2522%253A%257B%2522tabs%2522%253A%255B%255D%252C%2522id%2522%253A%2522clqcd4aok00033b5ws2bmdp62%2522%257D%257D%252C%2522showDevtools%2522%253Atrue%252C%2522showShells%2522%253Afalse%252C%2522showSidebar%2522%253Atrue%252C%2522sidebarPanelSize%2522%253A15%257D)

### Exercise, Thermostat

Here we have a thermostate that lets us se the temperature. We can also switch ebtween Celsius and Fahrenheit modes for displaying the current temperature.

With the current code, we are storin gtwo state variables, one for the `celsius` value and one for the `fahrenheit` value.

In this task, simplifythe code by having a single state variable that holds information about the temperature, as well as a 2nd state variable for displaying the format.

ACs:

- A single state variable should be used o keep track of the thermostat's current temperature.
- The 'up' and 'down' button should increase/decrease the current temperature by 1 degree.
  - Note: It doesn't matter what the current mode is. Whether it's in 'celcius' or 'fahrenheit' mode, these button shoudl increase/decrease the displayed temperature by `.
- You can use the helper functions `convertToCelsius` and `convertToFahrenheit` to convert between units.

- Notes:
- Well, here is the sandbox, getting a little deep in these exercises.
- Code and Box, [Deriving State - Thermostat Celcius to Fahrenheit](https://codesandbox.io/p/sandbox/hardcore-thompson-342vw2?layout=%257B%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522rootPanelGroup%2522%253A%257B%2522direction%2522%253A%2522horizontal%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522id%2522%253A%2522ROOT_LAYOUT%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522clqduhr8300063b5we3yvp0hk%2522%252C%2522sizes%2522%253A%255B100%252C0%255D%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522EDITOR%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522id%2522%253A%2522clqduhr8300023b5wr92vjch7%2522%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522SHELLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522id%2522%253A%2522clqduhr8300033b5wxuvdnk8l%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522DEVTOOLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522id%2522%253A%2522clqduhr8300053b5w9n7rojwl%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%252C%2522sizes%2522%253A%255B50%252C50%255D%257D%252C%2522tabbedPanels%2522%253A%257B%2522clqduhr8300023b5wr92vjch7%2522%253A%257B%2522id%2522%253A%2522clqduhr8300023b5wr92vjch7%2522%252C%2522tabs%2522%253A%255B%255D%257D%252C%2522clqduhr8300053b5w9n7rojwl%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clqduhr8300043b5w1cajwg0z%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522UNASSIGNED_PORT%2522%252C%2522port%2522%253A0%252C%2522path%2522%253A%2522%252F%2522%257D%255D%252C%2522id%2522%253A%2522clqduhr8300053b5w9n7rojwl%2522%252C%2522activeTabId%2522%253A%2522clqduhr8300043b5w1cajwg0z%2522%257D%252C%2522clqduhr8300033b5wxuvdnk8l%2522%253A%257B%2522tabs%2522%253A%255B%255D%252C%2522id%2522%253A%2522clqduhr8300033b5wxuvdnk8l%2522%257D%257D%252C%2522showDevtools%2522%253Atrue%252C%2522showShells%2522%253Afalse%252C%2522showSidebar%2522%253Atrue%252C%2522sidebarPanelSize%2522%253A15%257D)

## Breaking the Rules

In the 'Wordle' clone project, we hav a `gameStatus` state variable:

- Use your instincts as to whether or not to derive state. As you get more familiar and comfortable with React. You will know when to break the rules.
