# The Joy of React - Module 4 - Component API Design

- [Course Outline Notes](../course-notes.md)

## Component API Design

### Component Libraries

- A component library is a collection of low-level generic components. The LEGO bricks that every application needs. Things like buttons, date pickers and auto-pickers.

- This is mostly third-party component libraries. The most popular being Material UI.

- Josh doesn't recommend using these sorts of tools in most cases. Can be useful for prototyping, internal tools, but not well suited for production applications.

- Why not? Check out the blog post on the subject. [You don't need a UI framework.](https://www.smashingmagazine.com/2022/05/you-dont-need-ui-framework/)

- The most critical part, very few tech companies actually rely on these tools. If your goal is to become a better React developer, you will need to know how to build applications without relying on these tools.

- Most tech companies build their own first-party component libraries. At DigitalOcean, built [Walrus](https://www.digitalocean.com/blog/introducing-walrus-digitaloceans-reimagined-design), at Khan Academy, built [Wonder Blocks](https://khan.github.io/wonder-blocks/?path=/docs/overview--docs)

#### Usability and Accessibility

Tools that focus exclusively on usability and accessibility without prescribing a bunch fo styles.

- [Reach UI](https://reach.tech)
- [Headless UI](https://headlessui.com)
- [Radix Primitives](https://www.radix-ui.com)
- [React ARIA](https://react-spectrum.adobe.com/react-aria/)

Other mentions:

- [Chakra UI](https://chakra-ui.com)
- Tailwind

#### Design Systems

To understand why very few product-focused companies use third party component libraries. We need to talk about design systems.

- A design system is what gives a brand its unique identity. It is a collection of rules that govern how a product loos, feels and behaves. It includes tokens for colors, typography styles, and in-depth designs for specific UI elements.

- Design systems are built by designers using design software, like Figma or Sketch. They provide the documents to developers to implement components based on their designs.

- In terms of an analogy, the design system is like a recipe book. The component library is the set of prepared ingredients required, chopped onions, tomatoes and mushrooms. And the web application is the finished meal.

- **Here is the problem**, third party component libraries have their own design system baked in.

- Material UI for example, uses Google's 'Material Design' system. It's a very specific aesthetic, used by official Google applications across the web and Android.

- Now, third-party component libraries do allow us to customize/tweak the styles, but it would be incredibly difficult and annoying to try and swap out an  entire design system this way. Developers who have done this, have regretted it.

- 📣 the bottom line: very few tech companies rely on 'pre-styled' component libraries like Material UI, because they want complete control over the branding / aesthetic.

#### Application Structure

When building a first-party component library, it's common for the component library to be its own project. It has its own git repository, its own landing page, and its own documentation.

- This is useful when our components need to be shared across multiple projects, but honesty, it adds a ton of friction.
- Josh's practice, is to keep all the components, from the entire spectrum, in a single directory.
- For example; the components from this course platform.

![components](images/image-27.png)

- Components with a single purple asterisk *, lowest-level components.
- Moving up the spectrum, components marked with **, higher level components, like `AccountDropDown`
- Finally, high-level components marked with, ***, things like `AccountPage` which renders all the main content of the Account Page.

- Good way to work, it's a spectrum and all the components are at your finger tips.

- Also, some directories have multiple components, for example the `Account Page`.
- Allow you to keep all the related content together.
- Here, the `MyAccountInfo` component will never be used outside of `AccountPage` component, and so it doesn't make sense to clutter up my `/src/components` directory.

![source directory](images/image-28.png)

- Blog post on this topic, [Delightful React File Structure](https://www.joshwcomeau.com/react/file-structure/)
