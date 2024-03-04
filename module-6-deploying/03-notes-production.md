# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Building for Production

What are the differences between 'development mode' and 'production mode'.

- In 'development mode', React and Next are both optimized for the developer experience. We get things like:

➡️ Better error messages
➡️ Improved integration with React developer tools
➡️ React Strict Mode, helping us catch edge case issues by doing things like running our effects twice.

- In 'production mode', it is optimized for the user experience. Essentially, it is a slimmed down mode focused on performance. The JS bundles become way smaller, and React's render performance becomes much better.

- You can check how your app performs in 'production mode'. Will give you a much more accurate picture of what it's like to use your app.

- Video Summary:
- Using our simple app we created in the previous exercise.
- Step 1, is to stop the 'dev' server, in order for the build to compile correctly, you cannot run multiple terminals.
- Step 2, run the `npm run build` command, this will generate our application. Bundle and minify everything, you can see the results in the terminal. And the break down for each route, `/`, `/about`.
  - Where is this stuff built? Puts everything in the, '.next' directory. At any point in time you can delete it, and re-generate. This is not an area you are supposed to dig through.
  - Next does the static site generation, and pre-renders, does the SSR ahead of time.
- Step 3, the final script, `npm run start`, it serving everything from the 'build' process, and runs Next in production.
  - Now you can do some performance testing, see what the network performance is like.
  - In Chrome / DevTools / Network, be sure to throttle to slow down the network a little, now you are seeing the actual production bundles being downloaded.
  - ℹ️ This does not have a 'reload' if you make any changes, so you would have to make your change, re-run the build then run the 'start' command.

![next build](images/image.png)

- To Summarize:
- You can generate a production build by running `npm run build`.
- You can run a local production server by running `npm run start`.
- Take care to stop your dev server before running either of these commands.
