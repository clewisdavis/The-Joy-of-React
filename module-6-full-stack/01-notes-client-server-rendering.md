# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Client vs. Server Rendering

- How is it that you can see a fully built out site, with JS disabled?
- Get a fully formed HTML document that includes all the content on the page. This is known as, 'Server Side Rending', SSR

- 💡 Server Side Rending, SSR, when you visit a website, visit a website and press 'enter', it makes a request to your server.
- And instead of serving a static HTML file, we are going to dynamically create the HTML file, using React on the server.

- 📣 Generate the HTML, using React on the server, and then the client receives a fully ready to go HTML document.

- The benefit of this, is in the browser, you do not have to wait for all the JS to download. The compiled HTML document and content shows up really quickly.
