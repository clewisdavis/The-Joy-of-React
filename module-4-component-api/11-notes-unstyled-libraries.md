# The Joy of React - Notes

- [Course Outline Notes](../course-notes.md)

## Module 4

### Component API Design

#### Unstyled Component Libraries

- Earlier in this module, discussed how component libraries like React Boostrap and Material UI are NOT often used by product companies, because they come with their own built in design system.

- But as we know, building our own UI components from scratch can be surprisingly challenging. Libraries like Material UI come with a robust, battle tested components for things like modals, auto completes and dropdowns.

- Fortunately we can separate the baby from the bathwater with unstyled component libraries.

- These libraries focus on purely the mechanics and usability. Often, accessibility is a first class concern. They have no built in design system adn the few styles that re included can be easily overridden.

- ℹ️ Mix and Match!

- Because these libraries are unstyled, you don't need to pick one an stick with. We can 'mix and match' components from multiple libraries.
- 📣 But won't it bloat our JS bundles to depend on multiple component libraries?
- Nope, all the libraries we are talking about support 'tree shaking', which means our bundles only include the specific component we `import`. Might be some small additional cost but very minimal.

- Why would we mix and match?
- We can assemble our own bespoke collection of our favorite components.
- If the package we choose becomes unmaintained, we can gradually migrate to a newer solution, one component at a time. We don't have to drop everything and spend weeks moving everything over.

##### Reach UI

- [Reach UI](https://reach.tech)
- Not being actively maintained at the moment, and recommended that folks us a different option.
- And incompatible with React 18

##### Radix Primitives

- [Radix Primitives](https://www.radix-ui.com/primitives), unstyled accessible components, with a ton of components.
- Top recommendation for using this library.

##### Headless UI

- [Headless UI](https://headlessui.com)
- By the team at Tailwind Labs, intended to be used with Tailwind, but not required.
- Modest collection of components

##### Ariakit

- [Ariakit](https://ariakit.org)

##### React Aria

- [React Aria](https://react-spectrum.adobe.com/react-aria/)
- Not actually a component library, but a library of React custom hooks that can be used to build a component library. Created and maintained by Adobe.
- Adobe also has a separate library, React Spectrum, is a styled component library, using Adobe's design system, by using React Aria.

##### Base UI

- [Base UI](https://mui.com/base-ui/getting-started/)
- Built by the Material UI team, an unstyled version of Material UI.
- Material UI is one of the most popular component libraries, with Base UI you get all the benefits of that battle tested, without having to adopt any of hte Material UI styles.

#### Converting our Modal

- The strategy here, is to use the '[Compound Components](https://courses.joshwcomeau.com/joy-of-react/04-component-design/06-compound-components)' pattern we learned earlier.

- Why this direction, of wrapping the third-party library components?

1. We can choose a consistent prop API. In a larger app, we might use components from different libraries and packages, and they all have their own names and common props like `isOpen` and `handleDismiss`. If we create our own wrappers, you have total control over the interface, and we can make sure it's consistent.
2. We might decide later, to migrate to a different third-party component. This way we only have to change one file. And not a bunch of changes to every single component that has a modal.

- [Forked Sandbox to convert modal to headless UI](https://codesandbox.io/s/modal-headless-ui-fnh9cj?file=/Modal.js)

- 🤔 Think about this from the consumer side, how a developer might use the API, and the producer side.

#### Exercises, unstyled component libraries

Unstyled Component library exercises

##### Building a FAQ

- Let's suppose we are building a FAQs component.

- On the surface, this sort of UI component might seem straightforward, but there are a bunch of usability/accessibility concerns here. Rather than try to tackle them all ourselves, we will use the [Radix Primitive "Accordion"](https://www.radix-ui.com/primitives/docs/components/accordion) component.

Your mission is to use this component to implement the FAQs.

ACs:

- Using the Accordion Component from Radix Primitives, wire it up so that we see the 4 questions. Clicking the quesiton should reveal the answer.
- The component should be styled sot hat it matches the example above. The classes are provided in `FrequentlyAskedQuestions.module.css`, but you need to apply them to the correct answer.
- No key warnings should be present in the console.

- Forked Sandbox - [FAQs component](https://codesandbox.io/s/accordion-unstyled-component-radix-primitive-fzhwgm?file=/FrequentlyAskedQuestions.js)

- Tips:
- Don't forget to `map()` over the data, and apply a `key` for each item, 🤔 always forget about that. Just remember every time you have an element displayed over and over, like a list. That is your clue to use `map()`.
- Learn to read the docs, a skill on it's own, for the functionality you want.

```JAVASCRIPT
// FrequentlyAskedQuestions.js
import React from "react";
import * as Accordion from "@radix-ui/react-accordion";

import styles from "./FrequentlyAskedQuestions.module.css";
import { ChevronDown } from "react-feather";

function FrequentlyAskedQuestions({ data }) {
  return (
    <Accordion.Root type="single" collapsible="true">
      {data.map(({ id, question, answer }) => (
        <Accordion.Item key={id} value={id} className={styles.item}>
          <Accordion.Header>
            <Accordion.Trigger className={styles.trigger}>
              {question}
              <ChevronDown />
            </Accordion.Trigger>
          </Accordion.Header>
          <Accordion.Content className={styles.Content}>
            {answer}
          </Accordion.Content>
        </Accordion.Item>
      ))}
    </Accordion.Root>
  );
}

export default FrequentlyAskedQuestions;

```

- 🤔 Stretch Goal: See if you can get the `Chevron` to work when you click on an item. The trick here, is to look at the docs, and see if exposes the state in an API, `[data-state]` is open or closed. And you can use that in your css to trigger the icon style, rotation.
- [Radix, API, Item](https://www.radix-ui.com/primitives/docs/components/accordion#item)

```CSS
.trigger[data-state="open"] svg {
  transform: rotate(180deg);
}
```

#### Exercises, Building a Tooltip

- On this course platform, I have an `Asterisk` component that I use to tuck additional contextual information our of the way.
- In this exercise, we refactor using the "[Tooltip](https://www.radix-ui.com/primitives/docs/components/tooltip)" Component from Radix Primitives.

ACs:

- Focusing or hovering over the asterisk should show the tooltip, containing the provided `children`.
- Wen hovering, the tooltip should show after 200ms.

- 🤔 Tooltip or a popover? Tooltips, semantically, are meant to provide context for a button, it's a tip about a tool. A more semantically appropriate component for the Asterisk use case would be a [Popover](https://www.radix-ui.com/primitives/docs/components/popover). A popover is a generic UI element that 'pops over' something else.

- Forked Sandbox - [Radix Tooltip](https://codesandbox.io/s/tooltip-radix-primitives-lxcm2h?file=/Asterisk.js)

```JAVASCRIPT
import React from "react";
import * as Tooltip from "@radix-ui/react-tooltip";

import styles from "./Asterisk.module.css";

function Asterisk({ children }) {
  return (
    <Tooltip.Root>
      <Tooltip.Trigger className={styles.trigger}>*</Tooltip.Trigger>
      <Tooltip.Content className={styles.content}>
        <Tooltip.Arrow />
        {children}
      </Tooltip.Content>
    </Tooltip.Root>
  );
}

export default Asterisk;
```
