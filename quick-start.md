# Quick Start

[https://beta.reactjs.org/learn](https://beta.reactjs.org/learn)

Welcome to the React documentation! Here is an overview of what you can find on this site.

### You will learn

## Introduction

This is a tiny React app. To get your first taste of React, **edit the code below** and make it display your name:

### What is React?

React is a JavaScript library for building user interfaces.

React stands at the intersection of design and programming. **It lets you take a complex user interface, and break it down into nestable and reusable pieces called** [**“components”**](https://beta.reactjs.org/learn/your-first-component) **that fit well together.** If you have a programming background, this might remind you of making a program out of functions. If you’re a designer, this might remind you of composing a design out of layers. If you’re new to both disciplines, that’s okay! Many people get into them with React. Using React might also remind you of building a castle out of toy bricks. Sometimes, it’s even fun.

React does not prescribe how you build your entire application. It helps you define and compose components, but stays out of your way in other questions. This means that you will either pick one of the ecosystem solutions for problems like routing, styling, and data fetching, or [use a framework](https://beta.reactjs.org/learn/start-a-new-react-project) that provides great defaults.

### What can you do with React?

Quite a lot, really! People use React to create all kinds of user interfaces—from small controls like buttons and dropdowns to entire apps. **These docs will teach you to use React on the web.** However, most of what you’ll learn here applies equally for [React Native](https://reactnative.dev) which lets you build apps for Android, iOS, and even [Windows and macOS](https://microsoft.github.io/react-native-windows/).

If you’re curious which products you use everyday are built with React, you can install the [React Developer Tools](https://beta.reactjs.org/learn/react-developer-tools). Whenever you visit an app or a website built with React (like this one!), its icon will light up in the toolbar.

### React uses JavaScript

With React, you will describe your visual logic in JavaScript. This takes some practice. If you’re learning JavaScript and React at the same time, you’re not alone—but at times, it will be a little bit more challenging! On the upside, **much of learning React is really learning JavaScript,** which means you will take your learnings far beyond React.

Check your knowledge level with [this JavaScript overview](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A\_re-introduction\_to\_JavaScript). It will take you between 30 minutes and an hour but you will feel more confident learning React. [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript) and [javascript.info](https://javascript.info) are two great resources to use as a reference.

### Installation (optional)

## Learn React

There are a few ways to get started:

* If you’re **feeling impatient and learn by example,** head straight to [**Thinking in React**](https://beta.reactjs.org/learn/thinking-in-react). This tutorial doesn’t explain the syntax in detail, but it will give you an idea of what it feels like to build user interfaces with React.
* If you’re **familiar with the concepts and want to browse the available APIs**, check out [**the API reference**](https://beta.reactjs.org/reference).
* The rest of this documentation is organized in chapters that **introduce each concept step by step**—with many interactive examples, detailed explanations, and challenges to check your understanding. You don’t have to read them sequentially, but each next page assumes you’re familiar with concepts from the previous pages.

To save you time, we provide **a brief overview of each chapter** below.

### Chapter 1 overview: Describing the UI

React applications are built from isolated pieces of UI called [“components”](https://beta.reactjs.org/learn/your-first-component). A React component is a JavaScript function that you can sprinkle with markup. Components can be as small as a button, or as large as an entire page. Here, a _parent_ `Gallery` component renders three _child_ `Profile` components:

The markup in the example above looks a lot like HTML. This syntax is called [JSX](https://beta.reactjs.org/learn/writing-markup-with-jsx), and it is a bit stricter (for example, you have to close all the tags). Note that the CSS class is specified as `className` in JSX.

Just like you can pass some information to the browser `<img>` tag, you can also pass information to your own components like `<Profile>`. Such information is called [_props_](https://beta.reactjs.org/learn/passing-props-to-a-component). Here, three `<Profile>`s receive different props:

You might wonder why `className="avatar"` uses quotes but `src={imageUrl}` uses curly braces. In JSX, curly braces are like a [“window into JavaScript”](https://beta.reactjs.org/learn/javascript-in-jsx-with-curly-braces). They let you run a bit of JavaScript right in your markup! So `src={imageUrl}` reads the `imageUrl` prop declared on the first line and passed from the parent `Gallery` component.

In the above example, all the data was written directly in markup. However, you’ll often want to keep it separately. Here, the data is kept in an array. In React, you use JavaScript functions like `[map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)` to [renders lists](https://beta.reactjs.org/learn/rendering-lists) of things.

## Ready to learn this topic?

Read [**Describing the UI**](https://beta.reactjs.org/learn/describing-the-ui) to learn how to make things appear on the screen, including declaring components, importing them, writing JSX with the curly braces, and writing conditions and lists.

[Read More](https://beta.reactjs.org/learn/describing-the-ui)

### Chapter 2 overview: Adding interactivity

Components often need to change what’s on the screen as a result of an interaction. Typing into the form should update the input field, clicking “next” on an image carousel should change which image is displayed, clicking “buy” puts a product in the shopping cart. Components need to “remember” things: the current input value, the current image, the shopping cart. In React, this kind of component-specific memory is called [_state_](https://beta.reactjs.org/learn/state-a-components-memory).

You can add state to a component with a `[useState](https://beta.reactjs.org/reference/usestate)` Hook. Hooks are special functions that let your components use React features (state is one of those features). The `useState` Hook lets you declare a state variable. It takes the initial state and returns a pair of values: the current state, and a state setter function that lets you update it.

This `Gallery` component needs to remember two things: the current image index (initially, `0`), and whether the user has toggled “Show details” (initially, `false`):

Notice how clicking the buttons updates the screen:

State can hold complex values, too. For example, if you’re implementing a form, you can keep an object in state with different fields. The `...` syntax in the below example lets you [create new objects based on existing ones](https://beta.reactjs.org/learn/updating-objects-in-state).

You can also hold arrays in state. This lets you add, remove, or change things in a list in response to user interactions. Depending on what you want to do, there are [different ways to make new arrays from existing ones](https://beta.reactjs.org/learn/updating-arrays-in-state).

## Ready to learn this topic?

Read [**Adding Interactivity**](https://beta.reactjs.org/learn/adding-interactivity) to learn how to update the screen on interaction, including adding event handlers, declaring and updating state, and the different ways to update objects and arrays in state.

[Read More](https://beta.reactjs.org/learn/adding-interactivity)

### Chapter 3 overview: Managing state

You’ll often face a choice of _what exactly_ to put into state. Should you use one state variable or many? An object or an array? How should you [structure your state](https://beta.reactjs.org/learn/choosing-the-state-structure)? The most important principle is to **avoid redundant state**. If some information never changes, it shouldn’t be in state. If some information is received from parent by props, it shouldn’t be in state. And if you can compute something from other props or state, it shouldn’t be in state either!

For example, this form has a **redundant** `fullName` state variable:

You can remove it and simplify the code by calculating `fullName` while the component is rendering:

This might seem like a small change, but many bugs in React apps are fixed this way!

Sometimes, you want the state of two components to always change together. To do it, remove state from both of them, move it to their closest common parent, and then pass it down to them via props. This is known as [“lifting state up”](https://beta.reactjs.org/learn/sharing-state-between-components), and it’s one of the most common things you will do writing React code. For example, in an accordion like below, only one panel should be active at a time. Instead of keeping the active state inside each individual panel, the parent component holds the state and specifies the props for its children.

## Ready to learn this topic?

Read [**Managing State**](https://beta.reactjs.org/learn/managing-state) to learn how to keep your components maintainable, including how to structure state well, how to share it between components, and how to pass it deep into the tree.

[Read More](https://beta.reactjs.org/learn/managing-state)

## Next steps

This page was fast-paced! If you’ve read this far, you have already seen 80% of React you will use on daily basis.

Your next steps depend on what you’d like to do:

* Go to [Installation](https://beta.reactjs.org/learn/installation) if you’d like to set up a React project locally.
* Read [Thinking in React](https://beta.reactjs.org/learn/thinking-in-react) if you’d like to see what building a UI in React feels like in practice.
* Or, start with [Describing the UI](https://beta.reactjs.org/learn/describing-the-ui) to get a closer look at the first chapter.

And don’t forget to check the [API Reference](https://beta.reactjs.org/reference) when you need the API without the fluff!
