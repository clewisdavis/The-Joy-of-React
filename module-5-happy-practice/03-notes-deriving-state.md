# The Joy of React - Module 5 - Happy Practice

- [Course Outline Notes](../course-notes.md)

## Deriving State

- Best practice, store the minimum amount of information in state. And if something can be derived from state, it should be.

### Exercise, Compose a Tweet

In the older versions of Twitter, a number showed how many characters you had left to use in your message

- In the playground below, we have recreated the implementation but it's not as nice. Improve the code to improve it.

ACs:

- The number of characters remaining should be derived from the `message` sate, not stored in a state variable.
- No effects should be used.
