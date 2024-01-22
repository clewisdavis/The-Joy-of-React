# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Error Boundaries

Error boundaries allow you to keep track of unexpected code failures, putting a wall around a component and it's children to prevent the entire app from error out.

- You can wrap an error boundaries around an entire slice of the React tree. And it will catch any errors thrown in any sibling components, children, grandchildren, and so on...

```JAVASCRIPT
import React from 'react';

import Header from './Header';
import Ticker from './Ticker';
import Stories from './Stories';
import ErrorBoundary from './ErrorBoundary';

function App() {
  return (
    <>
      <Header />
      
      <ErrorBoundary fallback={
        <div className='error'>Stock ticker did not load</div>
      }>
        <Ticker />
      </ErrorBoundary>
      
      <main>
        <h1>Top Stories</h1>
        <Stories />
      </main>
    </>
  );
}

export default App;
```

- The `ErrorBoundary` component.

```JAVASCRIPT
import React from 'react';

class ErrorBoundary extends React.Component {
  // Equivalent to:
  // const [hasError, setHasError] = useState(false);
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    console.error(error);
    console.info({ errorInfo });
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || null;
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

- For more info, see the React docs - [Catching rendering errors with an error boundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)

- That is it for this module, think a lot of those concepts a little beyond what I want to do right now.
