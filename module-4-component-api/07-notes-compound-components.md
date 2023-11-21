# The Joy of React - Module 4 - Component API Design

- [Course Outline Notes](../course-notes.md)

## Component API Design

### Compound Components

You may find this pattern in the wild with third-party packages that look something like this:

```JAVASCRIPT
// Example from React Bootstrap
import Dropdown from 'react-bootstrap/Dropdown';

function UserButton() {
  return (
    <Dropdown>
      <Dropdown.Toggle>
        Actions
      </Dropdown.Toggle>

      <Dropdown.Menu>
        <Dropdown.Item href="/change-email">
          Change Email
        </Dropdown.Item>
        <Dropdown.Item href="/reset-pwd">
        Reset Password
        </Dropdown.Item>
        <Dropdown.Item href="/delete">
        Delete account
        </Dropdown.Item>
      </Dropdown.Menu>
    </Dropdown>
  );
}
```

- If you are not familiar with this pattern, looks strange. Why do some elements have dot in the names?
- This pattern is called compound components. And honestly, I'm not a big fan.

#### The big trick

Functions can have properties.

```JAVASCRIPT
function addNums(a, b) {
  return a + b;
}

addNums.hello = "world";

console.log(addNums.hello); // "world"
```

In JS, functions are secretly objects. We can assign properties to them, the same way we read/write the properties on an object.

This means we can attach one component to another, using the same notation:

```JAVASCRIPT
function Dropdown({ children }) {
  return (
    <div>{children}</div>
  );
}

function DropdownToggle({ children }) {
  return (
    <button>{children}</button>
  );
}
function DropdownMenu({ children }) {
  return (
    <nav>{children}</nav>
  );
}
function DropdownItem({ href, children }) {
  return (
    <a href={href}>{children}</a>
  );
}

Dropdown.Toggle = DropdownToggle;
Dropdown.Menu = DropdownMenu;
Dropdown.Item = DropdownItem;

export default Dropdown;
```

- You can hang a bunch of smaller components on one main component.
- JSX can obfuscate things a bit, see how these get compiled:

```JAVASCRIPT
// in JSX:
<Dropdown.Item href="/change-email">
  Change Email
</Dropdown.Item>

// Compiled to JS:
React.createElement(
  Dropdown.Item,
  { href: '/change-email' },
  'Change Email'
)
```

- We accessing the `Item` property on the `Dropdown` function, which resolves to the `DropdownItem` component defined above.

#### An alternative structure

Another way to structure things:

```JAVASCRIPT
function Dropdown({ children }) {
  return (
    <div>{children}</div>
  );
}

export function DropdownToggle({ children }) {
  return (
    <button>{children}</button>
  );
}
export function DropdownMenu({ children }) {
  return (
    <nav>{children}</nav>
  );
}
export function DropdownItem({ href, children }) {
  return (
    <a href={href}>{children}</a>
  );
}

export default Dropdown;
```

- Instead of hanging all of our secondary components on the `Dropdown` function, we are exporting them as named exports.
- Then on the consumer side, we can import and render them:

```JAVASCRIPT
import Dropdown, {
  DropdownToggle,
  DropdownMenu,
  DropdownItem,
} from 'hypothetical-react-bootstrap/Dropdown';

function UserButton() {
  return (
    <Dropdown>
      <DropdownToggle>
        Actions
      </DropdownToggle>

      <DropdownMenu>
        <DropdownItem href="/change-email">
          Change Email
        </DropdownItem>
        <DropdownItem href="/reset-pwd">
        Reset Password
        </DropdownItem>
        <DropdownItem href="/delete">
        Delete account
        </DropdownItem>
      </DropdownMenu>
    </Dropdown>
  );
}
```

- Some frameworks prefer this, because it simplifies the import statement. But the downsides are..

- Confusing syntax
- Loose bundler optimizations like tree shaking
- Compatibility issues like Next 13

#### DIY API design

meh
