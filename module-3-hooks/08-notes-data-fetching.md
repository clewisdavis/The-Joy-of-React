# The Joy of React - Module 3 - Course Notes

- [Course Outline Notes](../course-notes.md)

## React Hooks

### Data Fetching

- These lessons focus on how to use Fetch in a React context. A fair amount of "assumed" knowledge here.

- Some Primers:
- [Synchronous vs. Asynchronous code](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing)
- [Promises](https://web.dev/promises/)
- [Async / Await](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/15-async-await)
- [HTTP Methods](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/16-http-methods)
- [Fetch Basics](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/17-fetch)

#### Test API

- A sample back-end API for learning about network requests.
- A cheatsheet about this sample API:
  - All request are artificially slowed by 1-2 seconds, to give su the time to check loading states.
  - We can simulate errors by passing a query parameter, `simmulatedError=true`
  - For endpionts that return data, that data will often be randomized / faked.

#### Fetching an Event

- A contact form, let's see how to implement one:

- The starter form, the page does a full page refresh, not exaclty what we want to do.
- We want to direct the submit, to a specific endpiont, the sample one above.

- On the form, the first thing we need to do is create a `onSubmit={handleSubmit}` function on the form

```JAVASCRIPT
function handleSubmit(event) {
  // prevent the default refresh
  event.preventDefault();
}
```

- Now, using the `fetch` method, we can make a request. The endpoint is provided `ENDPOINT`

```JAVASCRIPT
function handleSubmit(event) {
  // prevent the default refresh
  event.preventDefault();

  // fetch request the endpoint
  fetch(ENDPOINT, {
    // supply the http method for the endpoint
    method: 'POST'
  })
}
```

- Supply the data to the endpoint, and use `JSON.stringify()` to send the data accross network.

```JAVASCRIPT
    fetch(ENDPOINT, {
      // supply the http method, defined in the endpoint
      method: 'POST',
      // supply the data, email and message
      // gatcha, you cannot send objects accross the network
      // you have to use JSON.stringify and pass the object
      body: JSON.stringify({
        email,
        message,
      })
    })
```

- When you run the code above, it makes the request, `fetch` is Promise based.
- You want to use the `async`, `await` keywords to the `fetch` request.
- And create a `response` variable.

```JAVASCRIPT
  // make a async function
  async function handleSubmit(event) {
    event.preventDefault();

    // await the fetch, and capture in a response variable
    const response = await fetch(ENDPOINT, {
      // supply the http method, defined in the endpoint
      method: 'POST',
      // supply the data, email and message
      // gatcha, you cannot send objects accross the network
      // you have to use JSON.stringify and pass the object
      body: JSON.stringify({
        email,
        message,
      })
    })
  }
```

- Then to get the body of the response, use `response.json()`, which you also have to `await`.
- Why do you have to await the response? The raw json could come in parts, or streaming, make sure you have that covered.

```JSON
    // get the response of the json, and also await
    // why await? we don't know if we have the raw content, it could be streaming, comes over multiple parts.
    const json = await response.json();
    console.log(json);
```

- Console.log the response when you submit the form.
- The full `handleSubmit` function to submit the data from the form to an API endpiont.

```JAVASCRIPT
  // make a async function
  async function handleSubmit(event) {
    event.preventDefault();

    // await the fetch, and capture in a response variable
    const response = await fetch(ENDPOINT, {
      // supply the http method, defined in the endpoint
      method: 'POST',
      // supply the data, email and message
      // gatcha, you cannot send objects accross the network
      // you have to use JSON.stringify and pass the object
      body: JSON.stringify({
        email,
        message,
      })
    })
    // get the response of the json, and also await
    // why await? we don't know if we have the raw content, it could be streaming, comes over multiple parts.
    const json = await response.json();
    console.log(json);
  }
```

- Full form component:
- Sent data to our backend API using `fetch`. We saw how to send a POST request, how to stringify the body, and how to validate we received teh correct response from the server.

```JAVASCRIPT
import React from 'react';

const ENDPOINT =
  'https://jor-test-api.vercel.app/api/contact';

function ContactForm() {
  const [email, setEmail] = React.useState('');
  const [message, setMessage] = React.useState('');

  const id = React.useId();
  const emailId = `${id}-email`;
  const messageId = `${id}-message`;

  // make a async function
  async function handleSubmit(event) {
    event.preventDefault();

    // await the fetch, and capture in a response variable
    const response = await fetch(ENDPOINT, {
      // supply the http method, defined in the endpoint
      method: 'POST',
      // supply the data, email and message
      // gatcha, you cannot send objects accross the network
      // you have to use JSON.stringify and pass the object
      body: JSON.stringify({
        email,
        message,
      })
    })
    // get the response of the json, and also await
    // why await? we don't know if we have the raw content, it could be streaming, comes over multiple parts.
    const json = await response.json();
    console.log(json);
  }

  return (
    <form onSubmit={handleSubmit}>
      <div className="row">
        <label htmlFor={emailId}>Email</label>
        <input
          required={true}
          id={emailId}
          type="email"
          value={email}
          onChange={(event) => {
            setEmail(event.target.value);
          }}
        />
      </div>
      <div className="row">
        <label htmlFor={messageId}>Message</label>
        <textarea
          required={true}
          id={messageId}
          value={message}
          onChange={(event) => {
            setMessage(event.target.value);
          }}
        />
      </div>
      <div className="button-row">
        <span className="button-spacer" />
        <button>Submit</button>
      </div>
    </form>
  );
}

export default ContactForm;
```

- The next part, is to improve the UX, and make the form more user friendly.

##### Loading, success, and error statuses

When submitting network request, we want to update the UI to indicate 3 different status:

➡️ Loading
➡️ Success
➡️ Error

- and Four differnt status, at any given time
idle | loading | success | error

- Here is how to update the form, to work with these statuses.

- 1. Create a new status variable, to tell us how things are going for every snapshot that we render for the form component.

And Four differnt status, at any given time, they cover most network request.
idle | loading | success | error

```JAVASCRIPT
  // Create a status hook
  // idle | loading | success | error
  const [status, setStatus] = React.useState('idle');
```

- 2. In the handle submit function, set the status to loading

```JAVASCRIPT
  async function handleSubmit(event) {
    event.preventDefault();

    // onSubmit, set status to loading
    setStatus('loading');

  }
```

- 3. How do you check the status, the response you get back from the server?

- You can use `response.status`:
  - `200` if went well
  - `400` if client error
  - `500` if a server error
  - `300` series error

- Good idea in theory, but in practice, should check the response itself. The data that we receive from the server.
- When you submit the form, you can get a preview of the data, in the console.log

```JAVASCRIPT
{
  ok: true, 
  dataReceived: {
    email: "hello@hello.com", 
    message: "this is a hello world"
    }
}
```

- We are going to use the `ok` property, to determine if the response is valid or not.
- In the `useEffect` hook, write a conditional to check the status of `json.ok`

```JAVASCRIPT
  // check the status response
  if (json.ok) {
    setStatus('success');
  } else {
    setStatus('error');
  }
```

- 📣 Now we can start using this `status` variable in our UI.

- First thing, add `disabled={status === 'loading'}` to the form fields, input and the button.
- This will disable the form, on submit, and once the response is complete, everythign becomes back to idle state.
- ℹ️ Preferred to disable the form, because it stops the user form accidentely double clicking the button and doing multiple submissions. It also means they cannot edit the fields while in process of submitting it. Any edits they make will not be received by the backend.

- But this isn't enough on it's own, to just disable. We can also changs the text in the button.
- The button will change to 'Submitting...' onSubmit.

![Form loading](images/image-7.png)

```JAVASCRIPT
  <button 
    disabled={status === 'loading'}>
    {status === 'loading' ? 'Submitting...' : 'Submit'}
  </button>
```

- What about the loading state? If the request resolves successfully? You can render something else, like, 'Message sent!' or something more informational.
- Add the logic to the component, just before the `return` the JSX for the form component.

![Message Sent](images/image-8.png)

```JAVASCRIPT
  // if success, you can return a message
  if (status === 'success') {
    return <p>Message sent!</p>
  }
```

- But what is user wants to send multiple messages, you can add the message to the component.

![Alt text](images/image-9.png)

```JAVASCRIPT
return (
  <form onSubmit={handleSubmit}>
    /* contact form JSX here */

    {status === 'success' && <p>Message sent!</p>}
  </form>
)
```

- And you should clear out the message field, you can do this when you check the status, `setMessage('')`.

![Clear out message](images/image-10.png)

```JAVASCRIPT
    // check the status response
    if (json.ok) {
      setStatus('success');
      // clear the form
      setMessage('');
    } else {
      setStatus('error');
    }
```

- What about error states? The test API, supports passign a qeury parameter, you can set to true, you get an error state

```JAVASCRIPT
const ENDPOINT =
  'https://jor-test-api.vercel.app/api/contact?simulatedError=true';
```

- Open up dev tools, and network tab. Submit it again, and you will see the request throw an 500 error

![500 error](images/image-11.png)

- Check the error status, and display a message.

![Error message](images/image-12.png)

```JAVASCRIPT
return (
  <form onSubmit={handleSubmit}>
    /* contact form JSX here */

    {status === 'success' && <p>Message sent!</p>}
    {status === 'error' && <p>Something went wrong!</p>}
  </form>
)
```

- For form validation, you can rely on the native validation for form elements.
- Full Code Sample:

```JAVASCRIPT
// contact form
import React from 'react';

const ENDPOINT =
  'https://jor-test-api.vercel.app/api/contact?simulatedError=true';

function ContactForm() {
  const [email, setEmail] = React.useState('');
  const [message, setMessage] = React.useState('');

  // Create a status hook
  // idle | loading | success | error
  const [status, setStatus] = React.useState('idle');

  const id = React.useId();
  const emailId = `${id}-email`;
  const messageId = `${id}-message`;

  // make a async function
  async function handleSubmit(event) {
    event.preventDefault();

    // onSubmit, set status to loading
    setStatus('loading');

    // await the fetch, and capture in a response variable
    const response = await fetch(ENDPOINT, {
      // supply the http method, defined in the endpoint
      method: 'POST',
      // supply the data, email and message
      // gatcha, you cannot send objects accross the network
      // you have to use JSON.stringify and pass the object
      body: JSON.stringify({
        email,
        message,
      })
    })
    // get the response of the json, and also await
    // why await? we don't know if we have the raw content, it could be streaming, comes over multiple parts.
    const json = await response.json();
    console.log(json);

    // check the status response
    if (json.ok) {
      setStatus('success');
      // clear the form
      setMessage('');
    } else {
      setStatus('error');
    }

  }

  return (
    <form onSubmit={handleSubmit}>
      <div className="row">
        <label htmlFor={emailId}>Email</label>
        <input
          required={true}
          disabled={status === 'loading'}
          id={emailId}
          type="email"
          value={email}
          onChange={(event) => {
            setEmail(event.target.value);
          }}
        />
      </div>
      <div className="row">
        <label htmlFor={messageId}>Message</label>
        <textarea
          required={true}
          disabled={status === 'loading'}
          id={messageId}
          value={message}
          onChange={(event) => {
            setMessage(event.target.value);
          }}
        />
      </div>
      <div className="button-row">
        <span className="button-spacer" />
        <button 
          disabled={status === 'loading'}>
          {status === 'loading' ? 'Submitting...' : 'Submit'}
        </button>
      </div>
      {status === 'success' && <p>Message sent!</p>}
      {status === 'error' && <p>Something went wrong!</p>}
    </form>
  );
}

export default ContactForm;
```

#### Fetching on Mount

In the previous lesson, we made a network request in response to an event, like submitting a form. But what about the initial view?

In this example, let's suppose we are buidling a weather app. We want to show the user what hte current temperature in their area:

![Weather App](images/image-13.png)

- We want to show the temperature immediately, righ when the component mounts.
- This is a thorny problem, might seem like a slight difference, fetchign on mount instead of fetching on event, but it opens up a whole can of worms.

- It's tricky, in order to fetch on mount, we generlly use the `useEffect` hook, and there are some gotchase around using async/await in an effect, as well as dealing with Strict Mode.

- But also, i fwe want to solve this problem in a robust, production ready way, there are all sorts of concerns we nee dto worry about:
  - Caching the response so that it can be reused by mulitple compoennts across the app.
  - revalidating the data so that it never becomes to stale.

- 🕳️ This is a rabit hole 😬

- As a result it has become standard in the community to use a tool to help wtih all this.
- The React docs, suggest using a package like [React Query](https://tanstack.com/query/v4/?from=reactQueryV3&original=https://react-query-v3.tanstack.com/) or [SWR](https://swr.vercel.app).

##### Intro to SWR

- Below is an MVP implementation of the weather app using SWR.

```JAVASCRIPT
import React from 'react';
import useSWR from 'swr';

const ENDPOINT =
  'https://jor-test-api.vercel.app/api/get-temperature';

async function fetcher(endpoint) {
  const response = await fetch(endpoint);
  const json = await response.json();
  
  return json;
}

function App() {
  const { data, error } = useSWR(ENDPOINT, fetcher);
  
  console.log(data, error);
  
  return (
    <p>
      Current temperature:
      {typeof data?.temperature === 'number' && (
        <span className="temperature">
          {data.temperature}°C
        </span>
      )}
    </p>
  );
}

export default App;
```

- Explanation of the code above.
- So how does this work?
- You can import 3rd party hooks, and use in your application.
- First thing to do, is to import, the `swr` hook into the app.

```JAVASCRIPT
  import useSWR from 'swr';
```

- The way it works, is we call the `useSWR()` hook and we give it two things:

- First, they key, unique identifier for the API route going to use, the endpoint itself, the location you want to make the network request to.

```JAVASCRIPT
  const ENDPOINT = 'https://jor-test-api.vercel.app/api/get-temperature';

  function App() {
    // swr hook
    const { data, error } = useSWR(ENDPOINT, fetcher);
  }
```

- Second, a fetcher function, tools like `swr` are meant to be agnostic when it comes to fetching data over a network. Here we are packaging it up in a function and we are passing this function to the library. The hook, `useSWR` is going to decide when to call this function, and what to do with the returned value.

```JAVASCRIPT
  const ENDPOINT = 'https://jor-test-api.vercel.app/api/get-temperature';

  // fetcher function
  async function fetcher(endpoint) {
  // making a request to endpoint
  const response = await fetch(endpoint);
  // getting it as json
  const json = await response.json();
  // and returning it
  return json;
}

  function App() {
    // swr hook
    const { data, error } = useSWR(ENDPOINT, fetcher);
  }
```

- Whatever you return from the `fetcher` function, will be stored in the `data` variable, when we call the `useSWR` hook.
- Once we get that data, we are gettign it from the `data`variable, from inside `useSWR` hook.
- And we are using it to control the rendered output, JSX.

```JAVASCRIPT
  return (
    <p>
      Current temperature:
      {typeof data?.temperature === 'number' && (
        <span className="temperature">
          {data.temperature}°C
        </span>
      )}
    </p>
  );
```

##### The entire flow

- Component mounts, we call the `useSWR()` hook, with a unique key `ENDPOINT`, and a function `fetcher`.

```JAVASCRIPT
  const { data, error } = useSWR(ENDPOINT, fetcher);
```

- SWR, is going to call the function, and provide the key as the first paramerter. `ENDPOINT`, and the paramater `endpoint` are the same, the reason we call it `endpoint`, is to keep the function generic, so you can reuse.

```JAVASCRIPT
  const ENDPOINT =
  'https://jor-test-api.vercel.app/api/get-temperature';

  async function fetcher(endpoint) {
  // making a request to endpoint
  const response = await fetch(endpoint);
  // getting it as json
  const json = await response.json();
  // and returning it
  return json;
}

  function App() {
    const { data, error } = useSWR(ENDPOINT, fetcher);
  }
```

- The data returned from the `return json`, is populated in the `data` variable.
- Then you can use it, inside your JSX, to control the render.

---

- 🤔 Seems like a lot, what are the benefits here?
- SWR, **Stale While Revalidate**, it's a http caching strategy
- The idea, is the information on the page, may grow stale if you spend any length of time on the page.
- If you change tabs, and come back, it's a good idea to fetch new data.
- The act of swithing tabs, and coming back, that triggered the revalidation.
- 🤔 Notice we don't see a loading spinner or anything, *because it chooses to show, the stale data.*
  - *We are revalidating the data, but while we are diong that, we are keeping the stale data. The UI always shows something*

- Trying to do this on your own is very hard. Why use a library. But the library makes all this configurable, can choose when you want this to happen.

- Full weather compoennt:

```JAVASCRIPT
import React from 'react';
// Import swr hook
import useSWR from 'swr';

const ENDPOINT =
  'https://jor-test-api.vercel.app/api/get-temperature';

async function fetcher(endpoint) {
  // making a request to endpoint
  const response = await fetch(endpoint);
  // getting it as json
  const json = await response.json();
  // and returning it
  return json;
}

function App() {
  const { data, error } = useSWR(ENDPOINT, fetcher);
  
  console.log(data, error);
  
  return (
    <p>
      Current temperature:
      {typeof data?.temperature === 'number' && (
        <span className="temperature">
          {data.temperature}°C
        </span>
      )}
    </p>
  );
}

export default App;
```

#### Loading and error states

Our MVP above doesn't include any loading or error states. How can we implement them with SWR.

##### Loading

- In addtition to providing a `data` value, the `useSWR` hook also tells su where or not the request is curretly loading. We can pull out the `isLoading` key:

```JAVASCRIPT
  const { data, isLoading } = useSWR(ENDPIONT, fetcher);
```

- `isLoading` is a boolean value. The initial value is `true`, and it flips to `false` once the fetch reqeust has completed.
- We can use tha tvalue to confidtionally render a loading UI like this:

```JAVASCRIPT
  // return a loading conditionally
  function App() {
    const { data, is Loading } = useSWR(ENDPOINT, fetcher);
 
    // conditionally return if loading
    if (isLoading) {
      return (
        <p>Loading</p>
      );
    }

    return (
    <p>
      Current temperature:
      {typeof data.temperature === 'number' && (
        <span className="temperature">
          {data.temperature}°C
        </span>
      )}
    </p>
  );
  }
```

##### Error

- To simulate the error, we can add a query parameter to the `ENDPOINT`:

```JAVASCRIPT
  const ENDPOINT =
  'https://jor-test-api.vercel.app/api/get-temperature?simulatedError=true';
```

- This will cause the server to return a 500 status code, instead of a 200. It will also return the following JSON:

```JAVASCRIPT
{
  "error": "This request returns an error, because the “simulatedError” query parameter was specified."
}
```

- 👀 [See JS Primer on HTTP Status codes](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/18-http-status-codes)

``` JSON
200 — Everything went smoothly!
401 - You're not allowed to access this resource
404 - The server can't find the resource you requested
500 - Something unexpected gone wrong with the server (eg. it's on fire)
```

- With SWR, hoever, neighe rof these thigns is sufficient to mark this as an error.
- Remember: we manage the network request! Our `fetcher` function is responsible for retrieving the data, and passign it along to SWR.
- If we want this to count as an error, we need to throw it:

```JAVASCRIPT
  async function fetcher(endpiont) {
    const responce = await fetch(endpoint);
    const json = await response.json();

    if(!json.ok) {
      throw json
    }

    return json;
  }
```

- With this change done, `data` will be undefined, and `error` will be the object we got back from the server.

---

##### ℹ️ Throwing an object?

It's conventional in JS to throw an Error like this:

```JAVASCRIPT
  if (!json.ok) {
    throw new Error('Some sort of error message');
  }
```

- When working with SWR, however, I prefer to throw the JSON object.

```JAVASCRIPT
  if (!json.ok) {
    throw json;
  }
```

- It's equally valid, and it means that `error` will be an object rathe rthan being an `Error` instance. This is easier to work with, can access the data associated with the error.

- Final implementation with loading and error states:

```JAVASCRIPT
// weather app, SWR, with loading and error states
import React from 'react';
import useSWR from 'swr';

// Remove "?simulatedError=true" to see the success state:
const ENDPOINT =
  'https://jor-test-api.vercel.app/api/get-temperature?simulatedError=true';

async function fetcher(endpoint) {
  const response = await fetch(endpoint);
  const json = await response.json();

  if (!json.ok) {
    throw json;
  }

  return json;
}

function App() {
  const { data, isLoading, error } = useSWR(ENDPOINT, fetcher);

  if (isLoading) {
    return <p>Loading…</p>;
  }

  if (error) {
    return <p>Something's gone wrong</p>;
  }

  return (
    <p>
      Current temperature:
      {typeof data?.temperature === 'number' && (
        <span className="temperature">
          {data.temperature}°C
        </span>
      )}
    </p>
  );
}

export default App;
```

---

#### Exercises, Data Fetching

##### Book Search

Suppose we are building a Good reds-type site, a place for users to discover new books. Our job is to implement a search feature:

In the sandbox, you will find you have a `<SearchResult>` component ready to b used to display the results. Your mission 'if you choose it' is to wire up the search form so that the results are fetched from our backend API. An endpoint has been provided.

Example request/response:

```JAVASCRIPT
// REQUEST:
GET '/api/book-search?searchTerm=winter'

// RESPONSE:
{
  "ok": true,
  "results": [
    {
      "isbn": "1234567890123",
      "name": "Winter's Orbit",
      "author": "Everina Maxwell",
      "coverSrc": "/image-path/cover.png",
      "abstract": "While the Iskat Empire has long dominated the system…"
    }
  ]
}
```

**Acceptance Criteria:**

- Subitting the form should make a GET request to the supplied API endpoint, passing along `searchTerm` as a query parameter.
- If there are search results, we should map over them, rendering a `<SearchResults>` element for each one.
- We should show the text "Searching..." while the search is in progress.
- Before the user has searched, we should show a paragraph containing teh text "Welcome to Book Search!"
- If there are no matching results, we should show a paragraph containing the text "No results".
- If teh API returns an error, we should show a paragraphy containing the text "Something went wrong!"
  - You can simulate an error by passing `simulatedError=true` as a query parameter.
- There should be no key warning in the console.

ℹ️ You don't have to usr SWR for this. SWR is primarily useful when fetching data on mount.

ℹ️ How to test:

- The backend API has a very small sample librry of about 20 books. Most commons search queries will return 0 results.
- Use these terms:
  - fifth - 1 result
  - a - 18 results
  - becky - 4 results
  - hello - 0 results

- Setup the handle function

```JAVASCRIPT
  //set up a handle function
  async function handleSearch(event) {
    // turn off defualt browser bahavior
    event.preventDefault();

    // create a new url variable
    const url = `${ENDPOINT}?searchTerm=${searchTerm}`

    const response = await fetch(url);
    // use the results
    const json = await response.json();

    // pass the results to state
    setSearchResults(json.results);
    
  }
```

- Map over the results

```JAVASCRIPT
    {
      // map over the results
      // add the ?, optional chaining operator, if searchResults is null or undefined
      // iT doesn't go any further
      searchResults?.map(result => (
        <SearchResult result={result} />
      ))
    }
```

- Set up new state for `idle` | `success` | `loading` | `error`

```JAVASCRIPT
  // state for the differ states
  // idle | success | loading | error
  const [status, setStatus] = React.useState('idle');
```

- In the `handleSearch`, add the logic for the `loading`, `success` and `error` cases.

```JAVASCRIPT
  //set up a handle function
  async function handleSearch(event) {
    event.preventDefault();

    // Set the status to loading when we submit the search
    setStatus('loading');

    // create a new url variable
    const url = `${ENDPOINT}?searchTerm=${searchTerm}`

    const response = await fetch(url);
    // use the results
    const json = await response.json();

    // check if the json has been successfuly parsed
    // json.ok, if truthy then it's a succcess
    if (json.ok) {
      // pass results to state
      setSearchResults(json.results);
      // also set status to success
      setStatus('success');
    } else {
      // otherwise, set to error
      setStatus('error');
    }
  }
```

- Look at the JSX, what we are rendering, should only be shown in the success case.

```JAVASCRIPT
      <main>
        {
          // only be shown in the success case
          status === 'success' && (
            <div className="search-results">
              <h2>Search Results:</h2>
              {
                // map over the results
                searchResults?.map(result => (
                  <SearchResult result={result} />
                ))
              }
            </div>           
          )
        }
      </main>
```

- This allows us to add other branches for the other status's, `idle`, `error`, `loading`.

```JAVASCRIPT
      <main>
        {
          // idle state
          status === 'idle' && (
            <p>Welcome to Book Search</p>
          )
        }
        {
          // loading state
          status === 'loading' && (
            <p>Searching...</p>
          )
        }
        {
          // error state
          status === 'error' && (
            <p>Something went wrong!</p>
          )
        }
        {
          // only be shown in the success case
          status === 'success' && (
            <div className="search-results">
              <h2>Search Results:</h2>
              {
                // map over the results
                searchResults?.map(result => (
                  <SearchResult result={result} />
                ))
              }
            </div>           
          )
        }
      </main>
```

- Don't forget to add the key to the `<SearchResult/>` component. You can use the `isbn` value from the result object. ISBN is just a unique identifier for books, which works for this case.

```JAVASCRIPT
<SearchResult 
  result={result}
  key={result.isbn}
/>
```

- And we need to test the error state. You can do this, by adding the query parameter to the `ENDPOINT`, `&simulatedError=true`

![Error Status](images/image-14.png)

```JAVASCRIPT
  const url = `${ENDPOINT}?searchTerm=${searchTerm}&simulatedError=true`;
```

##### No results found

- What about the case, when no results are found?

- When you have nested logic, it's really difficult to figure out, what is going on.
- Instead, you can create a new component for the search result list.
- A different way to structure the code, you have the standard, global 4 status's always use, `idle`, `loading`, `error`, `success`.
- And in the `success` variant, you have a component, that handles the 'no results' case.

- **Success branch logic:**

```JAVASCRIPT
    {
      // only be shown in the success case
      status === 'success' && (
        // now just render the SearchResultList
        <SearchResultList
          searchResults={searchResults}
        />
      )
    }
```

- The `SearchResultList` component:

```JAVASCRIPT
// Component for the Search Result List, pass in the searchResult as a prop
function SearchResultList({ searchResults }) {
  // check if not results and do an early return
  if (searchResults.length === 0) {
    return <p>No results</p>;
  }
  
  // if do have resutls, return our search results
  return (  
      // markup for the search results
      <div className="search-results">
          <h2>Search Results:</h2>
              {
                // map over the results
                searchResults?.map(result => (
                  <SearchResult 
                    result={result}
                    key={result.isbn}
                  />
                ))
              }
      </div>    
  )
}
```

- If this were a real app, would break apart and manage components in differ files.

---

- **Full App() component:**

```JAVASCRIPT
// App.js Book search app
import React from 'react';

import TextInput from './TextInput.js';
import SearchResult from './SearchResult.js';

/*
  API INSTRUCTIONS
  
  This endpoint expects a GET request,
  with a query parameter of `searchTerm`.
  Eg. `/api/book-search?searchTerm=winter`
  
  To simulate an error, attach the following
  query parameter: `simulatedError=true`
  
  To test the results, here are some suggested
  search terms:
  
    • `fifth` — 1 result
    • `a` — 18 results
    • `becky` — 4 results
    • `hello` — 0 results
*/

const ENDPOINT =
  'https://jor-test-api.vercel.app/api/book-search';

function App() {
  
  const [
    searchTerm,
    setSearchTerm,
  ] = React.useState('');
  
  const [
    searchResults,
    setSearchResults,
  ] = React.useState(null);
  
  // state for the differ states
  // idle | success | loading | error
  const [status, setStatus] = React.useState('idle');

  //set up a handle function
  async function handleSearch(event) {
    event.preventDefault();

    // Set the status to loading when we submit the search
    setStatus('loading');

    // create a new url variable
    const url = `${ENDPOINT}?searchTerm=${searchTerm}`

    const response = await fetch(url);
    // use the results
    const json = await response.json();

    // check if the json has been successfuly parsed
    // json.ok, if truthy then it's a succcess
    if (json.ok) {
      // pass results to state
      setSearchResults(json.results);
      // also set status to success
      setStatus('success');
    } else {
      // otherwise, set to error
      setStatus('error');
    }
    
  }

  return (
    <>
      <header>
        <form onSubmit={handleSearch}>
          <TextInput
            required={true}
            label="Search"
            placeholder="The Fifth Season"
            value={searchTerm}
            onChange={(event) => {
              setSearchTerm(event.target.value);
            }}
          />
          <button>Go!</button>
        </form>
      </header>

      <main>
        {
          // idle state
          status === 'idle' && (
            <p>Welcome to Book Search</p>
          )
        }
        {
          // loading state
          status === 'loading' && (
            <p>Searching...</p>
          )
        }
        {
          // error state
          status === 'error' && (
            <p>Something went wrong!</p>
          )
        }
        {
          // only be shown in the success case
          status === 'success' && (
            // now just render the SearchResultList
            <SearchResultList
              searchResults={searchResults}
            />
          )
        }
      </main>
    </>
  );
}

// Component for the Search Result List
function SearchResultList({ searchResults }) {
  // check if not results and do an early return
  if (searchResults.length === 0) {
    return <p>No results</p>;
  }
  
  // if do have resutls, return our search results
  return (  
      // markup for the search results
      <div className="search-results">
          <h2>Search Results:</h2>
              {
                // map over the results
                searchResults?.map(result => (
                  <SearchResult 
                    result={result}
                    key={result.isbn}
                  />
                ))
              }
      </div>    
  )
}

const EXAMPLE = {
  isbn: '9781473621442',
  name: 'A Closed and Common Orbit',
  author: 'Becky Chambers',
  coverSrc: 'https://sandpack-bundler.vercel.app/img/book-covers/common-orbit.jpg',
  abstract:
    "Lovelace was once merely a ship's artificial intelligence. When she wakes up in an new body, following a total system shut-down and reboot, she has no memory of what came before. As Lovelace learns to negotiate the universe and discover who she is, she makes friends with Pepper, an excitable engineer, who's determined to help her learn and grow.",
};

export default App;
```

#### Fetching the current user

In the sandbox, you are given starter code to an API endpoint that returns data about a user, like this:

```JAVASCRIPT
{
  "user": {
    "name": "Ahmed",
    "email": "me@ahmed1234.com",
  }
}
```

Your mission, if you choose it, is to fetch the data from the endpoint provided, and to manage both laoding and error states.

AC's:

- 1. You should use the provided `useSWR` hook to perform the request.
- 2. While the request is running, a spinner should be shown. A `Spinner` component has been provided for this purpose.
- 3. If the request succeeds, the `<UserCard>` component should be shown, populated with the data from the request.
- 4. If the request fails, you can show an error message ( a standard paragraph with "Something went wrong" will do).
  - You can simulate the error by passing `simulatedError=true` as a query parameter.

- ℹ️ Youc an refer to the final sandbox from the previous lesson as a template for making reqeust with the `useSWR` hook

##### AC 1, Make the request via SWR

- Set up the SWR hook in the component.

```JAVASCRIPT
function App() {
  // SWR Hook, Stale While Rendering
  const { data, error } = useSWR(ENDPOINT, fetcher);

  // rest of code here..
}
```

- Create the `fetcher` fuction to call the endpoint.

```JAVASCRIPT
const ENDPOINT =
  'https://jor-test-api.vercel.app/api/get-current-user';

// fetcher async function
async function fetcher(endpoint) {
  // make request, store it
  const response = await fetch(endpoint);
  // getting it as json
  const json = await response.json();
  // return it
  return json;
}
```

- `console.log` the `data` to see what you get back.

##### AC 2, Set up the spinner

- Add `isLoading` to the `useSWR` hook.

```JAVASCRIPT
  const { data, isLoading, error } = useSWR(ENDPOINT, fetcher);
```

- Create the condition, to check if loading.

```JAVASCRIPT
function App() {

  // SWR Hook, Stale While Rendering
  const { data, isLoading, error } = useSWR(ENDPOINT, fetcher);
  console.log(data);

  // check if loading, display Spinner component
  if (isLoading) {
    return <Spinner />;
  }

  // if an error, show error text
  if (error) {
    return <p>Something went wrong</p>
  }
  
  return (
    <UserCard name="Name here" email='Email' />
  );
}
```

##### AC 3, Show the data in the Card

- Update the `<UseCard />` component with the data, if successful.

```JAVASCRIPT
  return (
    <UserCard name={data.user.name} email={data.user.email} />
  );
```

##### AC 4, If the reqeust fails, show error message

- You can simulate the error by passing `?simulatedError=true` as a query parameter.
- In the `fetcher` funciton, add a condition to check if the json is NOT ok. Then use `throw` statement, to terminate the function.

```JAVASCRIPT
const ENDPOINT =
  'https://jor-test-api.vercel.app/api/get-current-user?simulatedError=true';

// fetcher async function
async function fetcher(endpoint) {
  // make request, store it
  const response = await fetch(endpoint);
  // getting it as json
  const json = await response.json();
  // check the response ok
  if (!json.ok) {
    throw json;
  }
  // return it
  return json;
}
```

- Add the condition to the JSX, to check `error` and display a message.

```JAVASCRIPT
  // if an error, show error text
  if (error) {
    return <p>Something went wrong!</p>;
  }
```

- Full App solution for fetching user data.

```JAVASCRIPT
// App.js
import React from 'react';
import useSWR from 'swr';

import UserCard from './UserCard.js';
import Spinner from './Spinner.js';

/*
  API INSTRUCTIONS
  
  This endpoint expects a GET request.
  
  To simulate an error, attach the following
  query parameter: `simulatedError=true`
*/

const ENDPOINT =
  'https://jor-test-api.vercel.app/api/get-current-user';

// fetcher async function
async function fetcher(endpoint) {
  // make request, store it
  const response = await fetch(endpoint);
  // getting it as json
  const json = await response.json();
  // check the response ok
  if (!json.ok) {
    throw json;
  }
  // return it
  return json;
}

function App() {

  // SWR Hook, Stale While Rendering
  const { data, isLoading, error } = useSWR(ENDPOINT, fetcher);
  console.log(data);

  // check if loading, display Spinner component
  if (isLoading) {
    return <Spinner />;
  }

  // if an error, show error text
  if (error) {
    return <p>Something went wrong!</p>;
  }
  
  // check if you have data
  if (data?.user) {
    return (
      <UserCard name={data.user.name} email={data.user.email} />
    );    
  }
}

export default App;
```

#### Async Effect Gotcha

- Imagine we want to fetch some data, on mount, and we don't have SWR. We will makea `fetch` request inside a `useEffect` hook.
- Using async/await, could try something like this:

```JAVASCRIPT
React.useEffect(async () => {
  const response = await fetch(ENDPOINT);
  const json = await response.json();

  setTemperature(json.temperature);
}, []);
```

- In order to use the `await` keyword, we need to be within a `async` function.
- Seems logical we make our effect callback async, but doesn't work that way.
- You get this warning:

```CODE
  Warning: useEffect must not return anything besides a function, which is used for clean-up.

  It looks like you wrote useEffect(async () => ...) or returned a Promise. Instead, write the async function inside your effect and call it immediately.
```

- We're not allowed to make the effect callback async. Instead, we need to create another funciton wihtint he effect. Here how to solve this problem:

```JAVASCRIPT
  React.useEffect(() => {
    // Create an async function within our effect:
    function runEffect() {
      const response = await fetch(ENDPOINT);
      const jjson = await response.json();

      setTemperature(json.temperature);
    }

    // Immediately clal this function to run the effect:
    runEffect();
  }, []);
```

- We move al the effect logic into this async function.
- Using a generic name `useEffect` instead of a specific name like `fetchTemperature`, to make it clear that we are moving everything related to that effect into this function.

- If our effect has a cleanup function, that function should NOT be included in our `runEffect` function:

```JAVASCRIPT
  React.useEffect(() => {
    async funtion runEffect() {
      // ... Effect logic here
    }

    runEffect();

    return () => {
      // ... Cleanup logic here
    }
  }, [])
```

- Libraries like SWR solve this problem for us.
- But you cannot always use a data-fetchign library. And might be some cases wher eyou want to `await` some sort of async operation.

- ℹ️ Why async effect callbacks are not allowed?
  - `async` funcitons always return a promise.
  - So when React runs the effect, it doesn't recieve a cleanup function, it recieves a promise.
  - To avoid this, React team decided effect callbacks, can't be async functions.
  - Instead, wrap async logic up in an async function. That way React receives the cleanup function immediately.
