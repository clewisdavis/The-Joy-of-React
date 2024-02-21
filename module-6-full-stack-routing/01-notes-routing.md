# The Joy of React - Module 6 - Full Stack React

- [Course Outline Notes](../course-notes.md)

## Routing

- History lesson, in the late 1980s, a computer scientist, Tim Berners-Lee, had a novel idea: Hypertext but for the internet.

- "Hypertext", is a document format that includes, hyperlinks, shortcuts that you can use to jump from one document to another.
- Been around for a long long time, an acedemic paper published in the 1940s, and could become part of Doug Engelbart's 1968 talk, known as the '[Mother of All Demos](https://youtu.be/yJDv-zdhzMY?si=hSGXl8ILUCUhu1fN)'.

- To this day, 'links' are a killer feature on the web. Any project you have to be able to construct multiple pages and link between them.

- In this section, go deeper into how routing works in Next. Clever tricks that Next does to improve performance, see how to move programmatically from one page to another, and how to dynamically generate pages based on the URL.

### Page Transitions

- We have seen how to create multiple routes in Next, by creating `page.js` components and placing them in subdirectories.
- We yet to see how to connect those routes with links!
- Seems simple, but an incredible amount of engineering behind the scenes.

- Forked Repo - [Next 13 Routing](https://github.com/clewisdavis/next-13-routing/tree/main)

- You have multiple pages, but when using an `<a>`, anchor tag, the network has to request all the
