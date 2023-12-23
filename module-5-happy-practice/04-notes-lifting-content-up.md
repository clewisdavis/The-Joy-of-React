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
