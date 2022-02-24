# Manipulating the DOM with Refs

[https://beta.reactjs.org/learn/manipulating-the-dom-with-refs](https://beta.reactjs.org/learn/manipulating-the-dom-with-refs)

Because React handles updating the [DOM](https://developer.mozilla.org/docs/Web/API/Document\_Object\_Model/Introduction) to match your render output, your components won’t often need to manipulate the DOM. However, sometimes you might need access to the DOM elements managed by React—for example, to focus a node, scroll to it, or measure its size and position. There is no built-in way to do those things in React, so you will need a [ref](https://beta.reactjs.org/learn/referencing-values-with-refs) to the DOM node.

### You will learn

* How to access a DOM node managed by React with the `ref` attribute
* How the `ref` JSX attribute relates to the `useRef` Hook
* How to access another component’s DOM node
* In which cases it’s safe to modify the DOM managed by React

## Getting a ref to the node

To access a DOM node managed by React, first, import the `useRef` Hook:

Then, use it to declare a ref inside your component:

Finally, pass it to the DOM node as the `ref` attribute:

The `useRef` Hook returns an object with a single property called `current`. Initially, `myRef.current` will be `null`. When React creates a DOM node for this `<div>`, React will put a reference to this node into `myRef.current`. You can then access this DOM node from your [event handlers](https://beta.reactjs.org/learn/responding-to-events) and use the built-in [browser APIs](https://developer.mozilla.org/docs/Web/API/Element) defined on it.

### Example: Focusing a text input

In this example, clicking the button will focus the input:

export default function Form() {

const inputRef = useRef(null);

function handleClick() {

inputRef.current.focus();

&#x20;

}

To implement this:

1. Declare `inputRef` with the `useRef` Hook.
2. Pass it as `<input ref={inputRef}>`. This tells React to **put this `<input>`’s DOM node into `inputRef.current`.**
3. In the `handleClick` function, read the input DOM node from `inputRef.current` and call `[focus()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus)` on it with `inputRef.current.focus()`.
4. Pass the `handleClick` event handler to `<button>` with `onClick`.

While DOM manipulation is the most common use case for refs, the `useRef` Hook can be used for storing other things outside React, like timer IDs. Similarly to state, refs remain between renders. You can even think of refs as state variables that don’t trigger re-renders when you set them! You can learn more about refs in [Referencing Values with Refs](https://beta.reactjs.org/learn/referencing-values-with-refs).

### Example: Scrolling to an element

You can have more than a single ref in a component. In this example, there is a carousel of three images and three buttons to center them in the view port by calling the browser `[scrollIntoView()](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView)` method the corresponding DOM node:

import { useRef } from 'react';

export default function CatFriends() {

const firstCatRef = useRef(null);

const secondCatRef = useRef(null);

const thirdCatRef = useRef(null);

function handleScrollToFirstCat() {

firstCatRef.current.scrollIntoView({

behavior: 'smooth',

block: 'nearest',

inline: 'center'

});

function handleScrollToSecondCat() {

secondCatRef.current.scrollIntoView({

behavior: 'smooth',

block: 'nearest',

inline: 'center'

});

function handleScrollToThirdCat() {

thirdCatRef.current.scrollIntoView({

behavior: 'smooth',

block: 'nearest',

inline: 'center'

});

### Accessing another component’s DOM nodes

When you put a ref on a built-in component that outputs a browser element like `<input />`, React will set that ref’s `current` property to the corresponding DOM node (such as the actual `<input />` in the browser).

However, if you try to put a ref on **your own** component, like `<MyInput />`, by default you will get `null`. Here is an example demonstrating it. Notice how clicking the button **does not** focus the input:

function MyInput(props) {

return \<input {...props} />;

}

export default function MyForm() {

const inputRef = useRef(null);

function handleClick() {

inputRef.current.focus();

Clicking the button will print an error message to the console:

> Warning: Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?

This happens because by default React does not let a component access the DOM nodes of other components. Not even for its own children! This is intentional. Refs are an escape hatch that should be used sparingly. Manually manipulating _another_ component’s DOM nodes makes your code even more fragile.

Instead, components that _want_ to expose their DOM nodes have to **opt in** to that behavior. A component can specify that it “forwards” its ref to one of its children. Here’s how `MyInput` can use the `forwardRef` API:

This is how it works:

1. `<MyInput ref={inputRef} />` tells React to put the corresponding DOM node into `inputRef.current`. However, it’s up to the `MyInput` component to opt into that—by default, it doesn’t.
2. The `MyInput` component is declared using `forwardRef`. **This opts it into receiving the `inputRef` from above as the second `ref` argument** which is declared after `props`.
3. `MyInput` itself passes the `ref` it received to the `<input>` inside of it.

Now clicking the button to focus the input works:

import { forwardRef, useRef } from 'react';

const MyInput = forwardRef((props, ref) => {

return \<input {...props} ref={ref} />;

export default function Form() {

const inputRef = useRef(null);

function handleClick() {

inputRef.current.focus();

In design systems, it is a common pattern for low-level components like buttons, inputs, and so on, to forward their refs to their DOM nodes. On the other hand, high-level components like forms, lists, or page sections usually won’t expose their DOM nodes to avoid accidental dependencies on the DOM structure.

### When React attaches the refs

In React, every update is split in [two phases](https://beta.reactjs.org/learn/render-and-commit):

* During **render,** React calls your components to figure out what should be on the screen.
* During **commit,** React applies changes to the DOM.

In general, you [don’t want](https://beta.reactjs.org/learn/referencing-values-with-refs) to access refs during rendering. That goes for refs holding DOM nodes as well. During the first render, the DOM nodes have not yet been created, so `ref.current` will be `null`. And during the rendering of updates, the DOM nodes haven’t been updated yet. So it’s too early to read them.

React sets `ref.current` during the commit. Before updating the DOM, React sets the affected `ref.current` values to `null`. After updating the DOM, React immediately sets them to the corresponding DOM nodes.

**Usually, you will access refs from event handlers.** If you want to do something with a ref, but there is no particular event to do it in, you might need an effect. We will discuss effects on the next pages.

### Best practices for DOM manipulation with refs

Refs are an escape hatch. You should only use them when you have to “step outside React.” Common examples of this include managing focus, scroll position, or calling browser APIs that React does not expose.

If you stick to non-destructive actions like focusing and scrolling, you shouldn’t encounter any problems. However, if you try to **modify** the DOM manually, you can risk conflicting with the changes React is making.

To illustrate this problem, this example includes a welcome message and two buttons. The first button toggles its presence using [conditional rendering](https://beta.reactjs.org/learn/conditional-rendering) and [state](https://beta.reactjs.org/learn/state-a-components-memory), as you would usually do in React. The second button uses the `[remove()` DOM API]\(https://developer.mozilla.org/en-US/docs/Web/API/Element/remove) to forcefully remove it from the DOM outside of React’s control.

Try pressing “Toggle with setState” a few times. The message should disappear and appear again. Then press “Remove from the DOM”. This will forcefully remove it. Finally, press “Toggle with setState”:

import {useState, useRef} from 'react';

export default function Counter() {

const \[show, setShow] = useState(true);

const ref = useRef(null);

onClick={() => {

setShow(!show);

onClick={() => {

ref.current.remove();

{show &&

Hello world

}

After you’ve manually removed the DOM element, trying to use `setState` to show it again will lead to a crash. This is because you’ve changed the DOM, and React doesn’t know how to continue managing it correctly.

**Avoid changing DOM nodes managed by React.** Modifying, adding children to, or removing children from elements that are managed by React can lead to inconsistent visual results or crashes like above.

However, this doesn’t mean that you can’t do it at all. It requires caution. **You can safely modify parts of the DOM that React has **_**no reason**_** to update.** For example, if some `<div>` is always empty in the JSX, React won’t have a reason to touch its children list. Therefore, it is safe to manually add or remove elements there.

### Recap

* Refs are a generic concept, but most often you’ll use them to hold DOM elements.
* You instruct React to put a DOM node into `myRef.current` by passing `<div ref={myRef}>`.
* Usually, you will use refs for non-destructive actions like focusing, scrolling, or measuring DOM elements.
* A component doesn’t expose its DOM nodes by default. You can opt into exposing a DOM node by using `forwardRef` and passing the second `ref` argument down to a specific node.
* Avoid changing DOM nodes managed by React.
* If you do modify DOM nodes managed by React, modify parts that React has no reason to update.

In this example, the button toggles a state variable to switch between a playing and a paused state. However, in order to actually play or pause the video, toggling state is not enough. You also need to call `[play()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/play)` and `[pause()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/pause)` on the DOM element for the `<video>`. Add a ref to it, and make the button work.

import { useState, useRef } from 'react';

export default function VideoPlayer() {

const \[isPlaying, setIsPlaying] = useState(false);

function handleClick() {

const nextIsPlaying = !isPlaying;

setIsPlaying(nextIsPlaying);

}

src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"

&#x20;

{isPlaying ? 'Pause' : 'Play'}

}
