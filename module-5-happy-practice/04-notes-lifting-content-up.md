# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Lifting Content Up

- Strategy, LIFTING CONTENT UP, when we take some React element, that is being rendered in a child, we lift it into the parent component and pass the element through a prop like `{children}`.

```JAVASCRIPT
// pass the <Article> element through, using the opening and closing tags syntax of <MainContent>
function ArticlePage({ articleSlug }) {
  return (
    <>
      <Header />
      
      <MainContent>
        <Article articleSlug={articleSlug} />
      </MainContent>
      
      <footer>
        Copyright “The News” Inc.
      </footer>
    </>
  );
}

// use children prop
function MainContent({ children }) {
  return (
    <main>
      <aside>
        <SocialSharingWidget />
      </aside>
      {children}
    </main>
  );
}
```

- Why does this work, the parent / child relationships in the DOM.
- The important pice here, is a thing in React called 'owners' and 'ownee'.
- 🤔 The idea, is most elements are 'owned' by a particular component.

- For example, the `<Header>` component is created by the `ArticlePage` because it is the on that created it via `React.createElement(Header)`

```JAVASCRIPT
function ArticlePage({ articleSlug }) {
  return (
    <>
      <Header />
      {/* React.createElement(Header) is called by ArticlePage, so it owns Header */}
      
      <MainContent>
        <Article articleSlug={articleSlug} />
      </MainContent>
      
      <footer>
        Copyright “The News” Inc.
      </footer>
    </>
  );
}
```

- The owner of an element gets to decide what the props are.
- By lifting the element up, now `ArticlePage` gets to decide what the props are.
- We create the element, then we pass the element to `<MainContent>`, then MainContent renders it in the `{children}` slot.
- 🤔 brian hurts a little

### Exercise, Art Store

In the sandbox below, we are building an e-commerce store for an artist.

The header has a cute shopping cart button and the button has a badge that increments as the user starts adding items to the cart.

The components are structured in the following hierarchy:

- `App`
- `Header`
- `CartButton`

Using what we learned in the previous lesson, restructure things so that `App` owns the `CartButton` component, without changing the DOM structure at all.

ACs:

- The `<CartButton>` element should be owned by the `App` component, rather than `<Header>`.
- The DOM structure should not be affected (the cart button should still be a child of the `<header>` DOM node).
- Bonus: Using what we learned in the "Leveraging Keys" lesson, up date the code so that the 'fade' animation re-triggers whenever the number changes.

Notes:

- Add an opening and close to the `<Header>` component. And add a `children` prop/slot to the component.

```JAVASCRIPT
import React from 'react';

import CartButton from './CartButton';

function Header({ children }) {
  return (
    <header>
      <h1>
        Pintor Famoso
        <span className="artist-title">
          Abstract Expressionist
        </span>
      </h1>
      {/* <CartButton numOfItems={numOfItemsInCart} /> */}
      {children}
    </header>
  );
}

export default Header;
```

- Create an opening and closing `<Header>` tag, And lift up the `<CartButton>` component inside of it.

```JAVASCRIPT
function App() {
  const [cartItems, setCartItems] = React.useState([]);

  console.log(cartItems);
  
  function addToCart(item) {
    setCartItems([...cartItems, item]);
  }
  
  return (
    <>
      {/* <Header numOfItemsInCart={cartItems.length} /> */}
      <Header>
        <CartButton numOfItems={cartItems.length} />
      </Header>
      <Shop paintings={DATA} addToCart={addToCart} />
    </>
  );
}
```

- For the animation, just add a `key` to the DOM element, to trigger a re-render and therefore making the animation re-run.

```JAVASCRIPT
function CartButton({ numOfItems }) {
  
  return (
    <a href="/" className="cart-button">
      <ShoppingCart />
      {numOfItems > 0 && (
        // Add the key attribute to trigger the nice animation
        <span key={numOfItems} className="cart-number">
          {numOfItems}
        </span>
      )}
    </a>
  );
}
```
