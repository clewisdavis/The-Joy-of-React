# Module 2 - Working with State

- [Course Outline Notes](course-notes.md)

## State Management Tools

- One of the most common questions asked: "Should we use something like Redux to manage global state for use? Or is React capable enough on it's own?

- And the answer is, it depends.

### Some background info

- In the early days, React would only be one piece of your front-end stack. The best practice was to combine React with something like Flux, Redux, or MobX.

- The idea was to use React for *local state*, and tools like Redux for *global state*.
- "Local state" is state tht is only needed in one particular part of the application. Like the value of a controlled form, input or button.
- "Global State" is for broader things, like data about the currently logged in user, or maybe the currently selected color theme. A single piece of global state might be used in a dozen different corners of the app.

- From 2014 to 2016 a number of packages were competing to be the "global state" partner for React. The outcome of that was [Redux](https://redux.js.org).

### Intro to Redux

- Summary, In Redux, the global state is represented as a single object that floats outside our React application. This state can't be directly edited; instead, Redux listens fro "actions", events that signify that something happening in our app.

- All the action are logged by Redux. For example, a log for a typical e-commerce app.
  - User logs in
  - User submits search form
  - Search results received from server
  - User clicks on "Page 2" of search results
  - Search results received from server
  - User adds "Coffee  Machine" item to cart.

- Each of these actions fires through Redux, and we can write some code that controls how these actions should affect the state.

- Like in React, state updates in Redux are immutable.

### Application Types

How to choose when to use Redux vs. native React. Most web apps fall into three categories.

1. Not much state
2. Mostly client state
3. Mostly server state

- Not Much State - This is mostly websites, static sites, news websites, and blogs fall into this category. They can still have a lot of local state, but minimal with global state.

- Mostly Client State - This is, things that used to be a desktop application. Photo shop, video editors, word processors, all fit in this category. Lots of complex global state. We are just doing a lot of state manipulation in the browser. These are client heavy apps.

- Mostly server State - Web apps that work primarily with server state. A clear example is an analytics dashboard: A bunch of complex data in the database, and our app fetches that data and presents it to the user. Apps that are interfaces that let users read and manipulate data that lives in a database somewhere. Most CRUD (Create, Read, Update, Delete) fit into this category, most SaaS apps, social networks, search engines, e-commerce sites.

### The right tool for the job

The challenges are different for each category.

- For "Not much state" category, using just React exclusively for all state works great, not much to worry about with global state.

- Category 2, mostly client state, Redux is good choice. Redux helps to keep the code simple as it becomes more complex. And has a lot of built in performance optimizations.

- Category 3, mostly server state, most all network related.
  - Fetching the right date at the right time.
  - Making sure the data doesn't grow stale by revalidating it (re-fetching) and customizing it.
  - Caching the data sot hat multiple components don't repeat requests unnecessarily.
  - Optimistic UI updates, so that we predict how network requests will resolve.
  - Pagination, requesting small slices of the data and letting the user pull new data as needed.

- And, Redux doesn't really address any of those things, Redux is a state management tool, does not help with network stuff.

#### Network tools

- Tools to help us mange network related challenges.
  - [Apollo](https://www.apollographql.com)
  - [react-query](https://tanstack.com/query/v3/)
  - [SWR](https://swr.vercel.app)

- If you are building a video game, or audio editor, Redux is great. Most likely you are building in either Category 1 (Not much state), or Category 2 (Mostly server state)

#### Mental Model to figure out which tool

1. If it's a static website, use React alone, with the help of context
2. If client-heavy app, use Redux for global state
3. If it's a server heavy app, use React + something to help with the network stuff. For this course platform, Josh uses's Vercel's SWR.

**However**, React has evolved over the past few years, you can build large, sophisticated apps without any other state-management libraries.

🤔 If you are new to react, spend a year or two getting comfortable building apps. Then expand to other tools.

### Redux Toolkit and RTK-Query

- [Redux Toolkit](https://redux-toolkit.js.org), an opinionated toolset that aims to sand down some of the rough edges of working with Redux.
-Josh prefers the "Classic Redux" without the toolkit.
- If already using Redux Toolkit, makes sense to use RTK Query.

### Bonus: Input Cheatsheet

- [Going to link to this](https://courses.joshwcomeau.com/joy-of-react/02-state/11-bonus-cheatsheet), to much to write up.
- Text Inputs
- Textareas
- Radio buttons
- Checkboxes
- Select
- Specialty Inputs, range, data, color
