# The Joy of React - Module 4 - Component API Design

- [Course Outline Notes](../course-notes.md)

## Component API Design

### Polymorphism

In order to build usable, accessible interfaces, it's important to understand the semantics of different HTML tags.

- For example; if an element can be clicked to perform an action in JS, it should be a button. Unless that action is to navigate the user to a new page, in which case, it should be an anchor `<a>`.

- When choose an HTML tag, it is much more important to focus on the semantics than the aesthetics.
- You should use a `<button>` even if you don't want it to look like a button.
- With CSS, we can strip away all those built in button stles.
- It is easier to remove a handful of CSS rules than it is to recreate all of the usability benefits built into the `<button>` tag.
- With that in mind, let's suppose our designer wants us to build the following UI.

![Data Table View](images/image-32.png)

- See the top right corner, actions the user can take
- Looks like links? It depends on whether clicking them changes teh URL or not.
- "Export All Data" doesn't sound like a link to me; I imagine it generating a .csv and emailing it to the user.

- So, we can build a `LinkButton` component. It's always going to look like a link, but it's going to be flexible in its implementation: it can either render a `<a>` tag or a `<button>` tag, depending on whether an `href` is supplied.

- [Check out the sandbox](https://codesandbox.io/s/wor88z?file=/LinkButton.js&utm_medium=sandpack)

ACs:

- The `LinkButton` component has an optional prop, `href`
- If an `href` is provided, `LinkButton` should render an `<a>` tag, otherwise it should render a `<button>` tag.

- One approach, you can add a `if` conditional statement to check if you have an `href` and then return the anchor.
- This approach works, but, the problem is, you have two separate branches that have to be maintained.

```JAVASCRIPT
import React from 'react';

import styles from './LinkButton.module.css';

function LinkButton({ href, children }) {
  // TODO: render an <a> tag if “href” is provided.
  if (href) {
    <a href={href} className={styles.button}>
      {children}
    </a>
  };

  return (
    <button className={styles.button}>
      {children}
    </button>
  )
}

export default LinkButton;
```

- Instead, use this concept called, **Polymorphism**, 🤔 the condition of occurring in several different forms.
- First, create a variable, `Tag` and it will hold either a anchor tag `a`, or a button tag `button`.
- And decide which of those to choose, based off the `href` property. `typeof` is equal to a `string` and if so, render a `a` or a `button`

```JAVASCRIPT
const Tag = typeof href === 'string' ? 'a' : 'button';
```

- 🤔 Here is the tricky part, now going to take the variable, `Tag` and going to render it, as it it was a React component. And add the `href` prop to it.
- Why doesn't it always have an `href` since you are adding one a prop? This is React doing some clean up for us, when the prop is resolved to `undefined`, React will strip it out, before it creates the element.

```JAVASCRIPT
  return (
    <Tag 
      className={styles.button}
      href={href}
      {...delegated}
    >
      {children}
    </Tag>
  )
```

- Full component, with **Polymorphism**

```JAVASCRIPT
import React from 'react';

import styles from './LinkButton.module.css';

function LinkButton({ href, children, ...delegated }) {
  // TODO: render an <a> tag if “href” is provided.
  const Tag = typeof href === 'string' ? 'a' : 'button';
  
  return (
    <Tag 
      className={styles.button}
      href={href}
      {...delegated}
    >
      {children}
    </Tag>
  )
}

export default LinkButton;
```

- 🤔 How is this actually working? Remember `React.createElement`.
- The `Tag` variable, is going to resolve to either, string `a` or string `button`.
- Why is this using an uppercase? `Tag`, if you did a lower case `tag` you would end up with an element rendered as `<tag>` in your HTML. So you cannot use a lower case.
- 📣 The JSX compiler, uses the case of the first letter, to determine if it should render a DOM primitive, `div`, `a` etc. Or a React component, `Tag`.
- The uppercase, tells JSX how to treat it, as a string, or as a variable.

```JAVASCRIPT
return (
  React.createElement(
    Tag,
    {
      href,
      className: styles.button,
      ...delegated
    },
    children
  )
)
```

### Exercises, Polymorphism

#### A List component

Suppose you are building a list component. There are two types of lists in HTML: unordered list `<ul>` and ordered list `<ol>`. Our list component should be able to render either.

ACs:

- The consumer should be able to specify whether they want to render an `ol` or a `ul`, by passing a prop to `List`.
- The component should restrict the user so that it can only render `ul` and `ol`, and not for example a `p` or a `button`.

- Code Sandbox - [List Component](https://codesandbox.io/s/728sr2?file=/List.js&utm_medium=sandpack)

- A more standardized approach, is the `as` prop. Use object destructuring to extract the `as` prop, rename it to `Tag`.
- 👀 more on the [Object Destructuring JS lesson](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/05-object-destructuring)

- A standard practice, derived from styled components, 'as' [polymorphic prop](https://styled-components.com/docs/api#as-polymorphic-prop)

```JAVASCRIPT
function App() {
  return (
    <main>
      <List
        // add 'as' as a prop
        as='ol'
      >
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
      </List>
    </main>
  );
}

export default App;
```

- But how do you make this work?
- In the `List` component, add `as` as a prop, then define your `Tag` and assign it to equal `as`.

```JAVASCRIPT
import React from 'react';

import styles from './List.module.css';

function List({ 
  // ol and ul
  // add 'as' prop
  as,
  className = '', 
  children, 
  ...delegated 
}) {

  // define Tag
  const Tag = as;
  
  return (
    <Tag
      {...delegated}
      className={`${styles.wrapper} ${className}`}
    >
      {children}
    </Tag>
  )
}

export default List;
```

- Use some JS features, to rename destructured keys in an object 🤔
- `as: Tag`

- Now, what happens if I omit the 'as' prop? You get an error. Can solve this by setting a default tag.
- 🤔 Lots of JS tricks here, destructuring, rename the variable, and then setting a default value.
- `as: Tag = 'ul'`

- Next problem, what happens if you pass another tag? Button for example; should only be able to render a `ul` and a `ol`.
- Throw an error, and interpolate the Tag.

```JAVASCRIPT
  // Check the tag
  if (!VALID_TAGS.includes(Tag)) {
    throw new Error(`Unrecognized tag: ${Tag}`);
  }
```

- Full solution:

```JAVASCRIPT
// List.js
import React from 'react';

import styles from './List.module.css';

// define the valid tags
const VALID_TAGS = ['ul', 'ol'];

function List({ 
  // ol and ul
  // reassigns the variable, then sets the default
  as: Tag = 'ul',
  className = '', 
  children, 
  ...delegated 
}) {

  // Check the tag
  if (!VALID_TAGS.includes(Tag)) {
    throw new Error(`Unrecognized tag: ${Tag}`);
  }
  
  return (
    <Tag
      {...delegated}
      className={`${styles.wrapper} ${className}`}
    >
      {children}
    </Tag>
  )
}

export default List;
```

```JAVASCRIPT
// App.js
import React from 'react';

import List from './List'

function App() {
  return (
    <main>
      <List
        as='ul'
      >
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
      </List>
    </main>
  );
}

export default App;
```

#### Exercise, Customized heading levels

HTML gives us 6 different heading tags, `<h1>` through `<h6>`.

Some developers believe that you are supposed to pick the one based on the size of the text: `<h1>` for large, `<h6>` for small ones. But, we can use CSS to make any heading tag any size. We should never pick HTML tags based on their aesthetics, we should pick them based on semantics.

- The goal with heading tags is to organize the content as a hierarchy.

- Hardcoding headings can be a challenge. Suppose you have this code:

```JAVASCRIPT
function SectionWithHeading({ heading, children, ...delegated }) {
  return (
    <section {...delegated}>
      <h2>{heading}</h2>
      {children}
    </section>
  );
}
```

- This component bundles together a chunk of stuff with a heading. But it hardcodes an `<h2>` tag, which might not always be the right heading level.
- You can fix that with polymorphism.

ACs:

- Update the `<SectionWithHeading>` component so that it allows the cunsumer to specify the heading level.
- In `App.js`, add a prop to each instance to set the heading level to the correct value.
- When properly set, styles will be applied automatically to the heading tags, and teh final results should look like this:

![Alt text](images/image-33.png)

- My solution:

```JAVASCRIPT
// SectionWithHeading.js
import React from 'react';

// define valid heading tags
const VALID_TAGS = ['h1', 'h2', 'h3', 'h4', 'h5'];

function SectionWithHeading({ 
  // add the as prop and assign to Tag with default value of h1
  as: Tag = 'h1',
  title, 
  children 
}) {

  // check valid heading tags
  if(!VALID_TAGS.includes(Tag)) {
      throw new Error(`Unrecognized tag: ${Tag}, valid tags are ${VALID_TAGS}`);
  }
  
  return (
    <section>
      <Tag>{title}</Tag>
      {children}
    </section>
  );
}

export default SectionWithHeading;
```

```JAVASCRIPT
//App.js
import React from 'react';

import SectionWithHeading from './SectionWithHeading';

function App() {
  return (
    <SectionWithHeading as='h1' title="George Mouzalon">
      <p>
        George Mouzalon (Greek: Γεώργιος Μουζάλων, romanized: Geōrgios Mouzalōn; c. 1220 – 25 August 1258) was a high official of the Empire of Nicaea under Theodore II Laskaris (r. 1254–1258).
      </p>
      <p>
        Of humble origin, he became Theodore's companion in childhood and was raised to high state office upon the latter's assumption of power. This caused great resentment from the aristocracy, which had monopolized high offices and opposed Theodore's policies. Shortly before Theodore's death in 1258, he was appointed regent of Theodore's under-age son John IV Laskaris (r. 1258–1261).
      </p>

      <SectionWithHeading as='h2' title="Biography">
        <SectionWithHeading
          as='h3'
          title="Early life and service under Theodore II"
        >
          <p>
            The Mouzalon family is first attested in the 11th century, but produced few notable members until the mid-13th century, with exception of Nicholas IV Mouzalon, Patriarch of Constantinople in 1147–1151.[1][2] George Mouzalon was born at Adramyttium on the western Anatolian coast in c. 1220.
          </p>
          <p>
            His family was considered as low-born, but he and his brothers became the childhood friends of the future Theodore II Laskaris, being raised with him in the palace as his paidopouloi (παιδόπουλοι, "pages"). It is assumed that they were also educated along with Theodore, sharing his classes under the scholar Nikephoros Blemmydes.[3][4][5] There were also at least two sisters, one of whom was later married to a member of the Hagiotheodorites family.[5]
          </p>
        </SectionWithHeading>
        <SectionWithHeading
          as='h3'
          title="Appointment as regent and assassination"
        >
          <p>
            Shortly before Theodore II died on 16 August 1258, he left George Mouzalon as regent and guardian of his eight-year-old son John IV. Patriarch Arsenios Autoreianos may have shared guardianship of John: although the later historians Nikephoros Gregoras and Makarios Melissenos say the Patriarch was named in this context, the contemporary historians George Pachymeres and George Akropolites name only Mouzalon.[16]
          </p>
          <p>
            This appointment further enraged the aristocracy, and Mouzalon's position became extremely precarious.[4][17][18] Mouzalon was also unpopular with the clergy because he was associated with Theodore's high-handed treatment of the Church, and with the people, who feared that he would try to usurp the throne.
          </p>
          <p>
            Most importantly, however, he faced the hostility of the army, in particular the Latin mercenaries, who had apparently been denied the usual stipends and donatives. In addition, they probably resented Theodore's intention to raise a "national" army composed solely of Byzantine Greeks, and Mouzalon is recorded by Pachymeres to have taken such measures.
          </p>
        </SectionWithHeading>
      </SectionWithHeading>
    </SectionWithHeading>
  );
}

export default App;
```

- An alternative solution to this, to create an `level` prop and just pass in the level number.

```JAVASCRIPT
// SectionWithHeading.js
import React from 'react';

function SectionWithHeading({
  level,
  title,
  children,
}) {
  if (
    typeof level !== 'number' ||
    level < 1 ||
    level > 6
  ) {
    throw new Error(
      `Unrecognized heading level: ${level}`
    );
  }

  const HeadingTag = `h${level}`;

  return (
    <section>
      <HeadingTag>{title}</HeadingTag>
      {children}
    </section>
  );
}

export default SectionWithHeading;
```
