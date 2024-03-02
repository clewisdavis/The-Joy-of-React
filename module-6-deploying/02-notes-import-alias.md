# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Import Aliases

One of the downsides to file based routing is tha twe tend to wind up with sprawling directory structures. You end up with something like:

```JAVASCRIPT
// /src/app/profiles/[:profileId]/page.js
import React from 'react';

import { getProfileInfo } from '../../../helpers';

async function ProfilePage({ params }) {
  // Stuff here
}

export default ProfilePage;
```

- As you can see, you have to count the number of `../` and get pretty annoying 😬. And makes it hard to move files around.
- The solution to that 😄, **Import aliases:**

- This comes built into Next, when you use a create-next-app, it looks like this:

```JAVASCRIPT
import { getProfileInfo } from '@/helpers';
```

The `@` is an alias for our root directory, `src`. This makes the import paths absolute instead of relative; and you can ove this file anywhere int eh codebase, and the import will still resolve correctly.

- Remember the question during the starter project process, you can customize the import alias.

```JAVASCRIPT
? Would you like to customize the default import alias? › No / Yes
```

- If you select 'Yes', you will be prompted to select a new character. In practice, most devs stick with teh default. The other popular options seen is `$`.

ℹ️ Config the alias

- How do you config the alias if you change your mind? This setting lives in a file called `jsconfig.json`:

```JAVASCRIPT
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

- This `jsconfig.json` file is used by IDEs like VS Code to enable things like auto completion. If you want to pick a different alias for your Next project, you can edit it here.

- Downsides: Import aliases are not actually JS, this is a custom thing at the bundler level, tools like Webpack, do essentially a find and replace during the build process for the real paths.
