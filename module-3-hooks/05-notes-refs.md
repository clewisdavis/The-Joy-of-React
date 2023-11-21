# The Joy of React - Module 3 - Course Notes

- [Course Outline Notes](../course-notes.md)

## React Hooks

### Refs

- How might we work with the `<canvas>` element in React?
- In this case, if you wanted to work with the `<canvas>`, you would have to get a context, a reference to that element.
- In JS, we do this by, using the `document.querySelector('canvas')` api, to get a reference to that element.
- But to do this in React, considered a bad practice, to reach around React, and get a reference to an element.
- This will also lead to bugs, when you have multiple instances of that component.

- To get the context, or reference of an instance, the conventional way is to use the `ref` attribute, hook.
- Its a little like the `key`, it signals to React we want to do something special.
- One way, is we can pass it a function, and React will provide us, with the underlying DOM node.

```JAVASCRIPT
function ArtGallery() {
  return (
    <main>
     <canvas
       ref={function(canvas) {
        console.log(canvas)
       }}
       width={200}
       height={200}
    />
    </main>
  )
}
```

- In the console,  you get the actual `<canvas>` element, the same thing you would get, if you called `document.querySelector('canvas')`
- This is one way to get to the underlying DOM node.

- The way `ref` works, it will be called whenever the component renders.

- A hook to use, in these situations, called `React.useRef()`
- The idea with `useRef()` is that it creates a box, and we can put whatever we want in the box, and React will thread this through every single render, so that I always have access to it.

- By default, `React.useRef()` creates an object, set to `{current: undefined}`
- Similar to the `React.useState()` hook, you can pass an initial value if you want.

```JAVASCRIPT
function ArtGallery() {
  const canvasRef = React.useRef();

  return (
    <main>
     <canvas
       ref={canvasRef}
       width={200}
       height={200}
    />
    </main>
  )
}
```

- You don't want to re-assign the variable, you want to mutate the `useRef` object.
- Hold up, aren't you not supposed to mutate object?
- That the difference, `React.useState()` is supposed to be immutable, BUT `React.useRef()` is intended to be mutated.

- Breakdown what doing:
- Creating a variable, `canvasRef` that starts as `{current: undefined}`
- Then you are capturing a reference, to a particular DOM node, by setting the `ref={canvasRef}` within the element, in this case, it's the `<canvas />` DOM node.
- This sets the `React.useRef()` hook to be set to the element, in this case `{ current: <canvas> }`

- Full Code Sample:

```JAVASCRIPT
import React from 'react';

function ArtGallery() {
  // 1. Create a “ref”, a box that holds a value.
  const canvasRef = React.useRef(); // { current: undefined }

  return (
    <main>
      <div className="canvas-wrapper">
        <canvas
          // 2. Capture a reference to the <canvas> tag,
          // and put it in the “canvasRef” box.
          //
          // { current: <canvas> }
          ref={canvasRef}
          width={200}
          height={200}
        />
      </div>

      <button
        onClick={() => {
          // 3. Pluck the <canvas> tag from the box,
          // and pass it onto our `draw` function.
          draw(canvasRef.current);
        }}
      >
        Draw!
      </button>
    </main>
  );
}

function draw(canvas) {
  const ctx = canvas.getContext('2d');

  ctx.clearRect(0, 0, 200, 200);

  ctx.beginPath();
  ctx.rect(30, 90, 140, 20);
  ctx.fillStyle = 'black';
  ctx.fill();
  ctx.closePath();

  ctx.beginPath();
  ctx.arc(100, 97, 75, 1 * Math.PI, 2 * Math.PI);
  ctx.fillStyle = 'tomato';
  ctx.fill();
  ctx.closePath();

  ctx.beginPath();
  ctx.arc(100, 103, 75, 0, 1 * Math.PI);
  ctx.fillStyle = 'white';
  ctx.fill();
  ctx.closePath();
  
  ctx.beginPath();
  ctx.arc(100, 100, 25, 0, 2 * Math.PI);
  ctx.fillStyle = 'black';
  ctx.fill();
  ctx.closePath();
  ctx.beginPath();
  ctx.arc(100, 100, 19, 0, 2 * Math.PI);
  ctx.fillStyle = 'white';
  ctx.fill();
  ctx.closePath();
}

export default ArtGallery;
```

### Exercises, Refs

- Video playback speed - we have a `<VideoPlayer> component that includes a playback speed control. But it doesn't work.
- For context, in vanilla JS, you can affect the playback speed of a `<video>` element with the following:

```JAVASCRIPT
const videoElement = document.querySelector('#some-video');
videoElement.playbackRate = 2; // Play at 2x speed
```

- AC's
- When the use changes the "Playback speed" control adn then plays the corresponding video, that video should play at the selected speed.
- You should use the `useRef` hook to capture a ref tot he `<video>` element.

- My attempt:

```JAVASCRIPT
import React from 'react';

function VideoPlayer({ src, caption }) {
  const playbackRateSelectId = React.useId();

  const videoRef = React.useRef(); // {current: undefined}
  console.log(videoRef);
  
  return (
    <div className="video-player">
      <figure>
        <video
          controls
          src={src}
          ref={videoRef}
        />
        <figcaption>
          {caption}
        </figcaption>
      </figure>
      
      <div className="actions">
        <label htmlFor={playbackRateSelectId}>
          Select playback speed:
        </label>
        <select
          id={playbackRateSelectId}
          defaultValue="1"
          onChange={(element) => {
            // grad the reference element
            const videoElement = videoRef.current;
            // grab the value of the select
            const videoRate = element.target.value;
            console.log(videoRate);
            // set the playback on the element and set it equal to the value of the select
            videoElement.playbackRate = videoRate;
          }}
        >
          <option value="0.5">0.5</option>
          <option value="1">1</option>
          <option value="1.25">1.25</option>
          <option value="1.5">1.5</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>
      </div>
    </div>
  );
}

export default VideoPlayer;
```

- Solution video:
- Same solution, but you could simplify the `onChange` within the select.

```JAVASCRIPT
onChange={(event) => {
  videoRef.current.playbackRate = event.target.value;
}}
```

- NOTE: Don't forget the `.current` after.

### Exercise, Media Player

- Let's build a media player!

- The UI is ready and we have loaded an audio file, using `<audio>` tag. Our job now is to capture a reference to that element, and to trigger it when the user clicks teh play/pause button.

- For context, here how we solve this problem in vanilla JS:

```JAVASCRIPT
const audioElement = document.querySelector('#some-audio-element');

// Start playing the song
audioElement.play();

// Stop paying the song
audioElement.pause();
```

- AC's
- Clicking the "Play" button should start playing the song.
- Clicking the button again should pause the song.
- By default, we should render a `<Play>` icon inside the button, but it should flip to a `<Pause>` icon while the song is playing.

- Hint: to keep track whether the song is currently playing or not, you should use React state variable.
- In general, if you want some part of the UI to update, you need to use `useState()` hook.

- My attempt:

```JAVASCRIPT
import React from 'react';
import { Play, Pause } from 'react-feather';

import VisuallyHidden from './VisuallyHidden';

function MediaPlayer({ src }) {

  // use state to manage flipping the pause / play button
  const [isPlaying, setIsPlaying] = React.useState(false);
  
  const audioRef = React.useRef(); // {current: undefined}
  
  return (
    <div className="wrapper">
      <div className="media-player">
        <img
          alt=""
          src="https://sandpack-bundler.vercel.app/img/take-it-easy.png"
        />
        <div className="summary">
          <h2>Take It Easy</h2>
          <p>Bvrnout ft. Mia Vaile</p>
        </div>
        <button
          onClick={() => {
            // use the state variable, set to false by default
            if (isPlaying) {
              audioRef.current.pause();
            } else {
              audioRef.current.play();
            }

            // flip the setState variable from true false or false true with the Logical NOT expression !
            setIsPlaying(!isPlaying);
                
          }}
        >
        
          { // use the isPlaying variable, to show which icon is shown
          isPlaying ? <Pause /> : <Play/>
          }
 
          <VisuallyHidden>
            Toggle playing
          </VisuallyHidden>
        </button>
        
        <audio 
          src={src}
          ref={audioRef}
        />
      </div>
    </div>
  );
}

export default MediaPlayer;
```

- Additional solution: When the song finishes, we need to slip the icon back to the initial state with `onEnded` added to the `<audio>` element.

```JAVASCRIPT
<audio
  ref={audioRef}
  src={src}
  onEnded={() => {
    setIsPlaying(false);
  }}
/>
```
