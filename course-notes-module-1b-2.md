# Module 1 - Components / Iteration

## Iteration Lesson

- [Course Outline Notes](course-notes.md)

### Iteration

- From a previous example; Building a CRM, we extracted a `ContactCard` component and used it for our 3 contacts:

```JAVASCRIPT
<ul>
  <ContactCard
    name="Sunita Kumar"
    job="Electrical Engineer"
    email="sunita.kumar@acme.co"
  />
  <ContactCard
    name="Henderson G. Sterling II"
    job="Receptionist"
    email="henderson-the-second@acme.co"
  />
  <ContactCard
    name="Aoi Kobayashi"
    job="President"
    email="kobayashi.aoi@acme.co"
  />
</ul>;
```

- This solution works, but we don't always have the data when we write the code.
- If we are building a CRM software, this data will be dynamic. Every user will have a separate set of contacts, and change over time. We can't hardcode.

- In React, we solve this by using iteration. We dynamically create these React elements by using raw JS.

#### Mapping Over Data

- Let's suppose the data from our CRM is held in an array.

```JAVASCRIPT
import ContactCard from './ContactCard';

const data = [
  {
    id: 'sunita-abc123',
    name: 'Sunita Kumar',
    job: 'Electrical Engineer',
    email: 'sunita.kumar@acme.co',
  },
  {
    id: 'henderson-def456',
    name: 'Henderson G. Sterling II',
    job: 'Receptionist',
    email: 'henderson-the-second@acme.co',
  },
  {
    id: 'aio-ghi789',
    name: 'Aoi Kobayashi',
    job: 'President',
    email: 'kobayashi.aoi@acme.co',
  },
];

function App() {
  return (
    <ul>
      <ContactCard
        name="Name goes here"
        job="Job goes here"
        email="Email goes here"
      />
    </ul>
  );
}

export default App;
```

- We want to create a `<ContactCard>` element for each of the contacts in the `data` array, passing in their name/job/email.

- In React, we use pure JS, there is no "React syntax" for doing this iteration.
- 🤔 HINT: You can render an array inside the JSX, React will unpack it for you. For a refresher, you can look at "[Array Iteration Methods](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/08-array-iteration-methods)" lesson.

- You can use the `map.()` method to iterate over the data

```JAVASCRIPT
const elements = data.map(contact => (
  console.log(contact.email)
));
```

- You can use, just vanilla JS, inside of React.
- Now, all you have to do is put in your component.
- Then use an expression slot `{elements}`, to render it in your `App()`

```JAVASCRIPT
import ContactCard from './ContactCard';

const data = [
  {
    id: 'sunita-abc123',
    name: 'Sunita Kumar',
    job: 'Electrical Engineer',
    email: 'sunita.kumar@acme.co',
  },
  {
    id: 'henderson-def456',
    name: 'Henderson G. Sterling II',
    job: 'Receptionist',
    email: 'henderson-the-second@acme.co',
  },
  {
    id: 'aio-ghi789',
    name: 'Aoi Kobayashi',
    job: 'President',
    email: 'kobayashi.aoi@acme.co',
  },
];

function App() {
  const elements = data.map(contact => (
    <ContactCard
      name={contact.name}
      job={contact.job}
      email={contact.email}
    />
  ));
  return (
    <ul>
      {elements}
    </ul>
  );
}

export default App;
```

- If you console.log(elements), you will see that you are creating an array of React elements, and inserting them.

- It is more common to put the JSX inline, so everything happens inline and easier to determine what is happening.
- We are going to `map()` over the data, and render a contact card for every contact we have.

```JAVASCRIPT
function App() {
  return (
    <ul>
      {data.map(contact => (
        <ContactCard
          name={contact.name}
          job={contact.job}
          email={contact.email}
        />
      ))}
    </ul>
  );
}
```

- You can do anything you can do in JS. Can use the JS you already know.

- 🎁 Bonus material: JS primer bonus
- [Arrow Functions](https://courses.joshwcomeau.com/joy-of-react/01-fundamentals/06.01-mapping?peek=%2Fjoy-of-react%2F10-javascript-primer%2F04-arrow-functions)
- [Array Iteration Methods](https://courses.joshwcomeau.com/joy-of-react/01-fundamentals/06.01-mapping?peek=%2Fjoy-of-react%2F10-javascript-primer%2F08-array-iteration-methods)

#### JSX inside JS inside JSX

- When iterating with React, it is not uncommon to wind up with a structure like this:

```JAVASCRIPT
<ul>
  {items.map(item => (
    <li>{item}</li>
  ))}
</ul>;
```

- On the second line, we use curly brackets to add some "vanilla JS", to our JSX. But then we are using JSX inside those curly brackets!
- The JSX compiler is able to resolve the "nested" JSX calls. The end result is something like this:

```JAVASCRIPT
React.createElement(
  'ul',
  {},
  items.map(item => (
    React.createElement(
      'li',
      {},
      item,
    )
  )),
);
```

#### Keys

- The good ole keys console warning.
- `Warning: Each child in a list should have a unique "key" prop.`

- When we give React an array of elements, we also need to help react otu b uniquely identifying each element.
- In our CRM application, we can fix this by using the `id` as the key prop.

```JAVASCRIPT
const data = [
  {
    id: 'sunita-abc123',
    name: 'Sunita Kumar',
    job: 'Electrical Engineer',
    email: 'sunita.kumar@acme.co',
  },
  // ✂️ Other contacts trimmed
];
function App() {
  return (
    <ul>
      {data.map(contact => (
        <ContactCard
          key={contact.id}
          name={contact.name}
          job={contact.job}
          email={contact.email}
        />
      ))}
    </ul>
  );
}
```

- Our contacts data array has a unique identifier, `id`.
- We pass that string onto the `key` property, to resolve the error.

#### Why are keys necessary?

- Why can't React figure this out on it's own?
- You need keys, to help react understand exactly how the data shifts over time.
- When updating the DOM, there are multiple ways to get from one UI to another. And if you give React unique identifiers, we can see how those things change 🤔
- [Video explanation](https://courses.joshwcomeau.com/joy-of-react/01-fundamentals/06.02-keys)

#### Using the array index as the key?

- What if you are not given a unique id for each contact, like the example above.
- A common approach is to use the array index. The index is provided when we call the `.map()` method.

```JAVASCRIPT
const todos = [
  'buy milk',
  'feed goldfish',
  'return library book',
];

function TodoList() {
  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index}>{todo}</li>
      ))}
    </ul>
  );
}
```

- As we iterate through teh array, we are grabbing each items index, and applying it as a key. The output in React is like:

```JAVASCRIPT
<li key={0}>buy milk</li>
<li key={1}>feed goldfish</li>
<li key={2}>return library book</li>
```

- This can be a bit risky, React keys are meant to be uniquely identity a bit of content.
- In the re-order case, you could end up with something like this.

```JAVASCRIPT
<li key={0}>feed goldfish</li>
<li key={1}>buy milk</li>
<li key={2}>return library book</li>
```

- This could cause some bugs in your app.
- A good rule, is to not use `key` with array index if the items can be reordered.

#### Key Rules

- Some rules that govern how keys should be used.

##### Top level element

- the `key` prop needs to be applied ot hte very top-level element within the `.map()` call.
- This example is incorrect.

```JAVASCRIPT
function NavigationLinks({ links }) {
  return (
    <ul>
      {links.map(item => (
        <li>
          <a
            key={item.id} // incorrect element
            href={item.href}
          >
            {item.text}
          </a>
        </li>
      ))}
    </ul>
  );
}
```

- From React's perspective, it has a group of `<li>` elements, does not dig any deeper at the child elements.
- To fix it, just move the `key` up to the `<li>`.

- When using fragments, it's sometimes required to switch to the long form `react.Fragment`, so you can apply the key.

```JAVASCRIPT
// 🚫 Missing key: 
function Thing({ data }) {
  return (
    data.map(item => (
      <>
        <p>{item.content}</p>
        <button>Cancel</button>
      </>
    ))
  );
}

// ✅ Fixed!
function Thing({ data }) {
  return (
    data.map(item => (
      <React.Fragment key={item.id}>
        <p>{item.content}</p>
        <button>Cancel</button>
      </React.Fragment>
    ))
  );
}
```

##### Not global

- Keys only have to be unique within their array, the `key` prop does not have to be globally unique  across the entire application.
- Each `.map()` cal produces a separate array, and so it's not a problem.

#### Exercises, Iteration

- Some exercises to get practice on what we went over.

#### Avatar selection

- Update the multiple avatars.
- AC's
  - Create an array that holds the data needed for all the avatars.
  - The array should be iterated over, creating an `<Avatar />` element for each item in the array.
  - Should be no key warnings in the console.

```JAVASCRIPT
// start
import Avatar from './Avatar';

function App() {
  return (
    <div className="avatar-set">
      <Avatar
        src="https://sandpack-bundler.vercel.app/img/avatars/001.png"
        alt="person with curly hair and a black T-shirt"
      />
      <Avatar
        src="https://sandpack-bundler.vercel.app/img/avatars/002.png"
        alt="person wearing a hijab and glasses"
      />
      <Avatar
        src="https://sandpack-bundler.vercel.app/img/avatars/003.png"
        alt="person with short hair wearing a blue hoodie"
      />
      <Avatar
        src="https://sandpack-bundler.vercel.app/img/avatars/004.png"
        alt="person with a pink mohawk and a raised eyebrow"
      />
    </div>
  );
}

export default App;
```

- AC 1. Create an array that holds the data needed for all the avatars.
- Note: since the src url is the same, good practice to just specify the name of the image as an `id`.

```JAVASCRIPT
const data = [
  {
    id: '001',
    alt: 'person with curly hair and a black T-shirt',
  },
  {
    id: '002',
    alt: 'person wearing a hijab and glasses',
  },
  {
    id: '003',
    alt: 'person with short hair wearing a blue hoodie',
  },
  {
    id: '004',
    alt: 'person with a pink mohawk and a raised eyebrow',
  },
];
```

- AC 2. The array should be iterated over, creating an `<Avatar />` element for each item in the array.
- Note: Use string interpolation with template strings for the image url.
- Use an arrow function on the map method. `(data) => (return stuff)`, don't forget about the rules of an arrow function.
- In the function return, you can just call the expression, with slot.

```JAVASCRIPT
function App() {
  // iterate over
  const element = data.map(avatar => (
    // console.log(data);
    <Avatar
      src={`https://sandpack-bundler.vercel.app/img/avatars/${avatar.id}.png`}
      alt={avatar.alt}
    />
  ));

  return (
    <div className="avatar-set">
      {element}
    </div>
  );
}
```

- Note, Josh's solution, you can put the code inline within the return. Not make a separate variable.

```JAVASCRIPT
function App() {
  return (
    <div className="avatar-set">
      {data.map(avatar => (
        // console.log(data);
        <Avatar
          src={`https://sandpack-bundler.vercel.app/img/avatars/${avatar.id}.png`}
          alt={avatar.alt}
        />
      ))}
    </div>
  );
}
```

- AC 3. No Key warnings.

```JAVASCRIPT
<Avatar
  src={`https://sandpack-bundler.vercel.app/img/avatars/${avatar.id}.png`}
  alt={avatar.alt}
  key={avatar.id}
/>;
```

- Getting fancy
- Use destructuring to simplify the return
- Arrow function, implicit return

```JAVASCRIPT
function App() {
  return (
    <div className="avatar-set">
      {data.map(({ id, alt }) => (
        <Avatar
          src={`https://sandpack-bundler.vercel.app/img/avatars/${id}.png`}
          alt={alt}
        />
      ))}
    </div>
  );
}
```

#### Shopping Cart

- Imagine building a shopping cart UI. You receive an array of items being held in teh card from the server.
- If an item is otu of stock, they cannot purchase it. Should be displayed separately.

- Started on, but two problems remain:
  - Need to show all the items in teh user's cart, not just the first one.
  - Need to show two separate tables. One for in-stock, and one for sold-out.

- AC's
  - Update the `CartTable` component to use iteration.
  - Make sure no `key` warnings in the console.
  - In `App`, we should be rendering two `CardTable` elements:
    - One for the in-stock elements
    - One for the out-stock elements below the "Sold Out" heading.

- AC 1. Update the `CartTable` component to use iteration.
- AC 2. Make sure no `key` warnings in console

```JAVASCRIPT
// CartTable.js
function CartTable({ items }) {
  // TODO: Map through “items”, creating 1 row
  // per item.

  const productRow = items.map(({
    id, imageSrc, imageAlt, title, price,
  }) => (
    // console.log(imageSrc)
    <tr className="cart-row" key={id}>
      <td>
        <img
          className="product-thumb"
          src={imageSrc}
          alt={imageAlt}
        />
      </td>
      <td>
        {title}
      </td>
      <td>
            $
        {price}
      </td>
    </tr>
  ));

  return (
    <table className="shopping-cart">
      <thead>
        <tr>
          <th />
          <th>Title</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>
        {productRow}
      </tbody>
    </table>
  );
}

export default CartTable;
```

- AC 3. In `App`, we should be rendering two `CardTable` elements:
  - One for the in-stock elements
  - One for the out-stock elements below the "Sold Out" heading.

- 🤔 TIP: Use the `filter()` method on the list of items. Pass that into your `<CardTable>` component.

- Pass that into your `<CardTable>` component.
- 🤔 TIP: You can invert the condition so it will return falsy `!item.inStock`.

```JAVASCRIPT

const items = [
  {
    id: 'hk123',
    imageSrc: 'https://sandpack-bundler.vercel.app/img/shopping-cart-coffee-machine.jpg',
    imageAlt: 'A pink drip coffee machine with the “Hello Kitty” logo',
    title: '“Hello Kitty” Coffee Machine',
    price: '89.99',
    inStock: true,
  },
  // other items here
];

function App() {
  const inStockItems = items.filter(
    item => item.inStock,
  );
  const outOfStockItems = items.filter(
    item => !item.inStock,
  );

  return (
    <>
      <h2>Shopping cart</h2>
      <CartTable items={inStockItems} />
      <div className="actions">
        <button>Continue checkout</button>
      </div>

      <h2>Sold out</h2>
      <CartTable items={outOfStockItems} />
    </>
  );
}

export default App;
```
