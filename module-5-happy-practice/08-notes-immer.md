# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Immer

One of the cardinal rules in React is that state is immutable. That is true whether we are talking about `useState` or `useReducer`.

More complex state is, the harder it is to make necessary changes without mutating anything.

Example, let's suppose you have a calendar app, and we have a array of `events`. It might look something like this:

```JAVASCRIPT
const [events, setEvents] = React.useState([
  {
    eventId: 'coffee-with-samantha',
    date: '2023-01-01T12:30:00.000Z',
    metadata: {
      invitees: [
        {
          name: 'Samantha',
          email: 'samboombox123@aol.com',
        },
      ],
    },
  },
  {
    eventId: 'focus-time',
    date: '2023-01-01T15:00:00.000Z',
    metadata: {
      notes: 'Time for me to focus!',
    },
  },
  {
    eventId: 'team-meeting',
    date: '2023-01-02T10:00:00.000Z',
    metadata: {
      notes: 'Weekly team catch-up call!',
      invitees: [
        {
          name: 'Sadb Fabian',
          email: 'sfabian@widgetco.com',
        },
        {
          name: 'Gerarda Nicomedes',
          email: 'gnicomedes@widgetco.com',
        },
        {
          name: 'Sagit Edvaldo',
          email: 'sedvaldo@widgetco.com',
        },
        {
          name: 'Denis Seppo',
          email: 'dseppo@widgetco.com',
        },
      ],
    },
  },
]);
```

- How would you remove one contact, one of the invitees without mutating any of the arrays or objects held in React state?

- Solution Code:

```JAVASCRIPT
// updateStage.js
function updateState(currentState) {
  // Pluck out some variables, to help us later on.
  const relevantEvent = currentState[2];
  const { invitees } = relevantEvent.metadata;

  // Create a new list of invitees using `filter`.
  // It should include all teammates except Gerarda.
  const nextInvitees = invitees.filter(
    invitee => invitee.email !== 'gnicomedes@widgetco.com'
  );

  // Create a brand-new team-meeting event, with
  // all the same stuff, but with this new list
  // of invitees.
  const nextEvent = {
    ...relevantEvent,
    metadata: {
      ...relevantEvent.metadata,
      invitees: nextInvitees,
    }
  };

  // Create a brand-new list of events, including
  // the first two unchanged events, as well as our
  // brand-new team-meeting event.
  return [
    ...currentState.slice(0, 2),
    nextEvent,
  ];
}
```

- [Playground - Immer](https://codesandbox.io/p/sandbox/immer-hznfh8?file=%2FupdateState.js%3A38%2C5&layout=%257B%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522rootPanelGroup%2522%253A%257B%2522direction%2522%253A%2522horizontal%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522id%2522%253A%2522ROOT_LAYOUT%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522clr4yafu500063b5wkfyousqn%2522%252C%2522sizes%2522%253A%255B70%252C30%255D%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522EDITOR%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522id%2522%253A%2522clr4yafu500023b5wk7wg01wv%2522%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522SHELLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522id%2522%253A%2522clr4yafu500033b5w6x6vjauw%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522DEVTOOLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522id%2522%253A%2522clr4yafu500053b5wuwa338k2%2522%257D%255D%252C%2522sizes%2522%253A%255B100%255D%257D%255D%252C%2522sizes%2522%253A%255B40%252C60%255D%257D%252C%2522tabbedPanels%2522%253A%257B%2522clr4yafu500023b5wk7wg01wv%2522%253A%257B%2522id%2522%253A%2522clr4yafu500023b5wk7wg01wv%2522%252C%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clr4yrpp200023b5v2152f9lc%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522FILE%2522%252C%2522initialSelections%2522%253A%255B%257B%2522startLineNumber%2522%253A38%252C%2522startColumn%2522%253A5%252C%2522endLineNumber%2522%253A38%252C%2522endColumn%2522%253A5%257D%255D%252C%2522filepath%2522%253A%2522%252FupdateState.js%2522%252C%2522state%2522%253A%2522IDLE%2522%257D%255D%252C%2522activeTabId%2522%253A%2522clr4yrpp200023b5v2152f9lc%2522%257D%252C%2522clr4yafu500053b5wuwa338k2%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522clr4yafu500043b5wh1elo9ek%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522UNASSIGNED_PORT%2522%252C%2522port%2522%253A0%252C%2522path%2522%253A%2522%252F%2522%257D%255D%252C%2522id%2522%253A%2522clr4yafu500053b5wuwa338k2%2522%252C%2522activeTabId%2522%253A%2522clr4yafu500043b5wh1elo9ek%2522%257D%252C%2522clr4yafu500033b5w6x6vjauw%2522%253A%257B%2522tabs%2522%253A%255B%255D%252C%2522id%2522%253A%2522clr4yafu500033b5w6x6vjauw%2522%257D%257D%252C%2522showDevtools%2522%253Atrue%252C%2522showShells%2522%253Atrue%252C%2522showSidebar%2522%253Atrue%252C%2522sidebarPanelSize%2522%253A15%257D)

- That is a lot of code, because we cannot mutate anything. This is where Immer comes into play.

### Immer 101
