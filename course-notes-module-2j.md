# Module 2 - Working with State

- [Course Outline Notes](course-notes.md)

## Project: Word Game

- In this project, we will create a clone of the popular game, Wordle.
- In Wordle, you get a new word each day, and it's your challenge to guess what the is, in six guesses or less.
- You use the colors of background, to figure out what should go where.
- Touches on a lot of the core concepts we learned in previous lessons.

### Strategies and Mindset

- This project is broken down into 5 discrete exercises. We will build the Wordle clone step by step, working up to the finished game. Each one with it's own solution, and video walkthrough.

- The flow of this project, for this project:

1. Follow the instructions in the project's README.md, attempt to solve the first exercise.
2. Once you have solved, or attempted to solve, watch the solution video for the first exercise.
3. Repeat steps 1 and 2 for all 5 exercises.

#### Growth mindset

- Struggling and failing is much more productive than effortlessly breezing through an easier challenge. In this exercise, you will feel frustrated to solve, try and reframe it as a successful learning experience. Failure is inherently frustrating.

## Local Development

- The project source is here, <https://github.com/joy-of-react/word-clone>

- This is a short lesson on getting the project running.

### Node, NPM and command line

- If new to modern front end development, you can check out the [tools of the trade](https://courses.joshwcomeau.com/joy-of-react/11-tools-of-the-trade/00-introduction) lessons.
- Covers the terminal, code editor, Node.js and npm.

- Modern terminal tools
  - [Hyper](https://hyper.is)
  - VS Code terminal editor, View > Terminal in the VS Code app
  - [iTerm2](https://iterm2.com)
  - [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=en-us&gl=us)

- Shell language, when you use the terminal and press enter. That command will be interpreted by the shell language. The environment running within the terminal language.

- **Bash**, is the most popular shell language. Most of the time, online instructions will be in Bash. It is the default shell language used by most Linux distributions.

- **Zsh**, most modern MacOS versions ship with Zsh instead of Bash. And are very similar and share almost all the same commands. Can be used interchangeably.

- If you are using either Linux or MaxOS, you are good to go. Your computer already has "industry standard" shell language. If on Windows, you have some setup work todo.

- ℹ️ Skip the $, in most instructions you will see a command like this. `$ npm install some-package`
- The dollar sign isn't meant to be included. You are meant to type everything after the dollar sign.
- In the Bash shell language, the `$` is the prompt character, shown to indicate that the terminal is ready to receive a command.

- ℹ️ Exiting Vi / Vim, these are a PITA to exit.
- To quit without saving, use these steps:
  - Press `Escape`
  - Press `:` this should add the prompt at the bottom of the terminal.
  - Type `q!`
- If you'd like to save before quitting, you'd type `wq` (Write Quit) instead of `q!`.

### Tip N' Tricks on Terminal

- **Toggling command**, The `-` character will toggle you in between working directories, `cd -` in the terminal, kinda like the previous button on your tv remote, you can jump back and forth.

- **Clearing the terminal**, you can type `clear` and erase all commands. You can also use the shortcut, `ctrl` + `L`.

- **Switching to the GUI file explorer**, in MacOS, you can just enter `open .` to open the finder window for that directory.

- **Chaining commands**, you can chain two commands together, using the `&&` operator. The first command runs, then the second automatically. For example; `npm install && npm run start`

- **Terminal tiling and tabs**, when running a dev server, it is a long running process and is not able to accept additional commands. Modern terminal apps, you can run many terminal sessions in the same application. MacOS, the shortcut is `Shift` + `Command` + `d`.

- To run a new tab, just do `Command` + `t`

- ℹ️ Tip, you can use a window position manager, like [Rectangle](https://rectangleapp.com) or Moom

- ℹ️ VS Code Tip, to open VS Code from the MacOS terminal, `code .`, first you need to set it up in VS Code.
  - In VS Code, open the command palette with `Command` + `Shift` + `P`
  - Type `Shell` in the command palette
  - Select `Shell Command: Install code in PATH`

- ℹ️ VS Code Tip, to view the markdown, run the command palette, `Command` + `Shift` + `P`, and type, `markdownopen` to view a rendered view of the markdown. or `Markdown: Open Preview`

### Running the local dev server

- Download the app from git, <https://github.com/joy-of-react/word-clone>
- cd into the project
- Run `npm install`
- And run the app, `npm run start`
- View the `README.md`

## Getting Started

- Fork the repo, on your own github, so you can submit it and review the solution videos.
- The README.md is the file you want to follow, to go through each exercise.

- My repo, forked copy: <https://github.com/clewisdavis/joy-of-react-word-clone>

- In Wordle, users have 6 attempts to guess a 5-letter word. You are helped along the way by ruling out letters that are not in the word, and being told whether the correct letters are in teh correct location or not.

### Exercise 1: GuessInput

- First thing, we need a way to submit guesses.
- We need to render a little form that holds a text input:
- Create a new component for this UI, and render it inside the `Game` component.

- **Acceptance Criteria:**
- Create a new component, don't forget to use an NPM script to generate the scaffolding for you.
- This component should render a `<form>` tag, including a label and a text input.
- The text input should be controlled by React state.
- When the form is submitted:
  - The entered value should be logged to the console for now.
  - The input should be reset to an empty string.
- The user's input should be converted to ALL UPPERCASE. No lower case letters allowed.
- The input should have a minimum and maximum length of 5.
- Note: The `minLength` validator is a bit funky; you may wish to use the `pattern` attribute instead. This is discussed in mor detail on the Solution page.

- Solution, GuessInput Component

```JAVASCRIPT
import React from "react";

function GuessInput() {

  const [guess, setGuess] = React.useState('');

  function handleSubmit(event) {
    // Prevent the default browser behavior 
    event.preventDefault();
    // log to console, the {} renders in console as a object
    console.log({ guess });
    // reset the input
    setGuess('');
  }

  return (
    <>
     <form 
       className="guess-input-wrapper"
       onSubmit={handleSubmit}
      >
        <label htmlFor="guess-input">Enter guess:</label>
        <input 
          required
          minLength={5}
          maxLength={5}
          pattern="[a-zA-Z]{5}"
          title="5 letter word"
          id="guess-input"
          type="text"
          value={guess}
          onChange={ (event) => {
            const nextGuess = event.target.value.toUpperCase();
            setGuess(nextGuess);
          }}
          />
    </form>
    </>
  );
}

export default GuessInput;
```

## Exercise 2: Keeping track of the list

- The goal of this exercise is to render each of the user's guesses:

- Sample DOM structure

```JAVASCRIPT
<div class="guess-results">
  <p class="guess">FIRST</p>
  <p class="guess">GUESS</p>
</div>
```

- **Acceptance Criteria:**

- 1. A new component should be created, to render previous guesses.
- 2. When the user submits their guess, that value should be rendered within this new component.
- 3. There should be no key warnings in the console!

- Video Solution Notes:
- This exercise is tricky, seems simple, but how do you manage the state that we already have.
- The standard solution, to access data from one component to another, is to lift the state up.
- But, that does not work in this case. In this game, we only want to show the guess once it is submitted.
- When you go down the path of just lifting the state up. You see the guess as you type it, in the input field.
- What we want, the moment you type hello, and press enter, you see the guess. Transfer it, leaves the input and shows up in the list.

- Instead, you want to track, juggle the data between two bits of state.
- Create some new state, on the parent component `<Game/>`, that starts as an empty array.

```JAVASCRIPT
  const [guesses, setGuesses] = React.useState([]);
```

- Then write a new handler on the input `<GuessInput handleSubmitGuess={handleSubmitGuess}` and write the new function.
- This function will take the guess as an argument and update the guess to include in the array.

```JAVASCRIPT
  function handleSubmitGuess(guess) {
    // TODO
  }
```

- In the `<GuessInput/>` component, pass the handler in as an argument. So it can receive it.

```JAVASCRIPT
  // GuessInput.js
  function GuessInput({ handleSubmitGuess }) {
    // Component here
  }
```

- 🚀 In the `onSubmit` handler, for the input, pass that value up to the parent component, `Game.js`.

```JAVASCRIPT
  // GuessInput.js
  function handleSubmit(event) {
    // pass the value of guess up
    handleSubmitGuess(guess);
  }
```

- In the parent component, `<Game/>`, you can console.log out the guess to make sure the data is being passed from one component to another.

```JAVASCRIPT
  // Game.js
  function handleSubmitGuess(guess) {
    // Check to see if the data is being passed.
    console.log(guess);
  }
```

- In the handler, you need to add the new guess that was submitted, into the state, array. `guesses`
- Call the `setGuesses()` method to push the new guess into the array.
- Remember the rule, you are not allowed to modify existing arrays or objects.
- So we have to spread the current guess, and add the new guess

```JAVASCRIPT
  // Game.js
  function handleSubmitGuess(guess) {
    // Set state method, spread syntax, creating an array that includes all the current guesses. And add the new guess. 
    // Creates a new array, similar to array.push(), instead of modifying an existing
    setGuesses([...guesses, guess]);
  }
```

- JS Primer on the [spread syntax](https://courses.joshwcomeau.com/joy-of-react/10-javascript-primer/12-rest-spread)

- Pass the array of guesses to the `<GuessResults guesses={guesses} />` component, so you can display it.
- Then we need to map over the guesses.

- Is this a bad pattern, to copy between state? Not in this case, only when you have the exact same information, that is spread across multiple state.
- But here, we have state in the input, then when you hit enter, it is being transferred to the parent level array, the list.
- We are transferring the source of truth.

- Now to render the guesses using `map()`, map over the state

```JAVASCRIPT
  function GuessResults({ guesses }) {
    return (
      <div className="guess-results">
        // map over state
        {guesses.map(guess => (
          <p className="guess">{guess}</p>
        ))}
      </div>
    )
  }
```

- Now you need to apply a unique key prop to each guess.
- You can use `index` for key, but be aware, of the re-order issue. Does you application allow for re-order.

```JAVASCRIPT
  function GuessResults({ guesses }) {
    return (
      <div className="guess-results">
        // add the key, using index
        {guesses.map((guess, index) => (
          <p key={index} className="guess">{guess}</p>
        ))}
      </div>
    )
  }
```

- What if you cannot use the `index` for the key, if the order changes or swap?
- You would have to create a unique identifier with each guess. And that happens in your `handleSubmitGuess()` handler.

```JAVASCRIPT
  // Game.js
  function handleSubmitGuess(tentativeGuess) {

    // Create an object to hold the value and the unique id
    const nextGuess = {
      value: tentativeGuess,
      id: Math.random(), // set a unique id, 15 digits
    }
    // set that in the array for state, so now every item is going to be an object
    setGuesses([...guesses, nextGuess]);
  }
```

- Now in your `<GuessResults />` component.
- You can deconstruct that `{value, id}` and then use it on the component.

```JAVASCRIPT
  function GuessResults({ guesses }) {
    return (
      <div className="guess-results">
        // add the key, using index
        {guesses.map(({value, id}, index) => (
          <p key={id} className="guess">{value}</p>
        ))}
      </div>
    )
  }
```

## Exercise 3: Guess Slots

- Hint: You will need to generate a bunch of squares for each guess. This can be done using the provided [range utility](https://courses.joshwcomeau.com/joy-of-react/01-fundamentals/08-range-util).

- In this exercise, we will update our code to display 6 rows of guesses, no matter how many guesses the user has submitted, and each row will consist of 5 cells.
- As the user submits guesses, their guess will populate the cells:

- Starter markup for the slots

```HTML
<div class="guess-results">
  <p class="guess">
    <span class="cell">F</span>
    <span class="cell">I</span>
    <span class="cell">R</span>
    <span class="cell">S</span>
    <span class="cell">T</span>
  </p>
  <p class="guess">
    <span class="cell">G</span>
    <span class="cell">U</span>
    <span class="cell">E</span>
    <span class="cell">S</span>
    <span class="cell">S</span>
  </p>
  <p class="guess">
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
  </p>
  <p class="guess">
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
  </p>
  <p class="guess">
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
    <span class="cell"></span>
  </p>
</div>
```

- Things to know to help with this exercise:

1. You can use the `range` utility to create array sof a specified length to map over. It's provided in `/src/utils.js`. Check out range utility lesson for more info.
2. Inside `/src/constants.js`, you find a constant, `NUM_OF_GUESSES_ALLOWED`. You should import and use this constant when generating the set of guesses. This would make it easy for us to change the difficulty of the game later on, bu increasing/decreasing the # of guesses.

- Acceptance Criteria:
  - Create a new `Guess` component. 6 instances should be rendered at all times, no matter how many guesses have been submitted.
  - The `Guess` component should render 5 spans, each with the class of `cell`.
  - Each cell should contain a letter, if the `Guess` instance has been given a value. If not, the cell should be blank.
  - Use the `NUM_OF_GUESSES_ALLOWED` constant, when needed.
  - No `key` warnings in the console.

- Create a new component, but how do you split up the guesses and put them in each box?

```JAVASCRIPT
    function Guess({ value }) {
      return (
        <>
          <p className="guess">
            {
              // split up the guesses and map through
              value.split('').map((letter, index)) => (
                <span key={index} className="cell">
                  {letter}
                </span>
              )
            }
          </p>
        </>
      )
    }
```

## Exercise 4: Game logic

- In this exercise, we will add some CSS classes to color the background of each cell, based on the results and the correct answer:

- Inside `/src/game-helpers.js` you will find a helper function, `checkGuess`. As parameters, it takes a single guess, as well as the correct answer. It returns an array that contains the status for each letter.

- For example:

```JAVASCRIPT
checkGuess('WHALE', 'LEARN');
/*
  Returns:

  [
    { letter: 'W', status: 'incorrect' },
    { letter: 'H', status: 'incorrect' },
    { letter: 'A', status: 'correct' },
    { letter: 'L', status: 'misplaced' },
    { letter: 'E', status: 'misplaced' },
  ]
*/
```

- There are 3 possible statuses:
  - correct - this slot is perfect.
  - misplaced - this letter does exist in the word, but in a different slot.
  - incorrect - this letter is not found in the word at all.

- In the example above, `W` and `H` are not found in the word `LEARN`, and so they are marked as "incorrect". `A` is correct, since it is in the 3rd slot in each word. The other two letters `L` and `E` are meant to be in other slots.

- These statuses correspond with CSS classes. the `correct` status has a `correct` class name, which will apply the green background when applied to a cell. Same thing for `misplaced` and `incorrect`.

- The task is to use this function to validate the user's guesses, and apply the correct CSS classes. The final output for a given guess should look like this:

```HTML
<p class="guess">
  <span class="cell incorrect">W</span>
  <span class="cell incorrect">H</span>
  <span class="cell correct">A</span>
  <span class="cell misplaced">L</span>
  <span class="cell misplaced">E</span>
</p>
```

- Acceptance Criteria:
  - Import the `checkGuess` function from `/src/game-helpers.js` and use it to validate each of the user's guesses.
  - When rendering the letter in the `Guess` component, apply the letter's `status` to the `cell` element.
  - Empty guess slots should have the same markup as before: `<span class="cell"></span>`

## Exercise 5: Winning and loosing

- The last thing to do, is to end the game.
- If the user wins the game, a happy banner should be shown:
- If the user loses teh game, by contrast, a sad banner should be shown:

- The user wins teh game when their guessed word is identical to the `answer`. They lose the game if they submit six guesses without winning.

- When the game is over, one of these banners should be shown, and the text input should be disabled sot hat no new guesses can be typed or submitted.

**- Acceptance Criteria:**

- If the user wins the game, a happy banner should be shown.
- If the user loses the game, a sad banner should be shown.
- When teh game is over, the text input should be disabled.
- It is up to your to decide hwo to structure the banner. Feel free to create a new component if you think it's warranted.
