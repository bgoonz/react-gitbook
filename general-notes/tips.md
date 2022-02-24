---
cover: ../.gitbook/assets/react.png
coverY: 95.88377723970945
---

# Tips



The [key](https://facebook.github.io/react/docs/multiple-components.html#dynamic-children) is an attribute that you must pass to all components created dynamically from an array. It's a unique and constant id that React uses to identify each component in the DOM and to know whether it's a different component or the same one. Using keys ensures that the child component is preserved and not recreated and prevents weird things from happening.

> Key is not really about performance, it's more about identity (which in turn leads to better performance). Randomly assigned and changing values do not form an identity [Paul O’Shannessy](https://github.com/facebook/react/issues/1342#issuecomment-39230939)

* Use an existing unique value of the object.
* Define the keys in the parent components, not in child components

```javascript
//bad
...
render() {
	<div key=<div data-gb-custom-block data-tag="raw">{{item.key}}</div>><div data-gb-custom-block data-tag="raw">{{item.name}}</div></div>
}
...

//good
<MyComponent key=<div data-gb-custom-block data-tag="raw">{{item.key}}</div>/>
```

* [Using array index is a bad practice.](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318#.76co046o9)
* `random()` will not work

```javascript
//bad
<MyComponent key=<div data-gb-custom-block data-tag="raw">{{Math.random()}}</div>/>
```

* You can create your own unique id. Be sure that the method is fast and attach it to your object.
* When the number of children is large or contains expensive components, use keys to improve performance.
* [You must provide the key attribute for all children of ReactCSSTransitionGroup.](http://docs.reactjs-china.com/react/docs/animation.html)



You've been building your React apps with a Redux store for quite a while, yet you feel awkward when your components update so often. You've crafted your state thoroughly, and your architecture is such that each component gets just what it needs from it, no more no less. Yet, they update behind your back. Always mapping, always calculating.

### Reselector to the rescue

How could would it be if you could just calculate what you need? If this part of the state tree changes, then yes, please, update this.

Let's take a look at the code for a simple TODOs list with a visibility filter, specially the part in charge of getting the visible TODOs in our container component:

```javascript
const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
  }
}

const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}
```

Each time our component is updated, this needs be recalculated again. Reselector actually allows us to give our `getVisibleTodos` function memory. So it knows whether the parts of the state we need have changed. If they have, it will proceed, if they haven't, it will just return the last result.

Let's bring selectors!

```javascript
const getVisibilityFilter = (state) => state.visibilityFilter
const getTodos = (state) => state.todos
```

Now that we can _select_ which parts of the state we want to keep track of, we're ready to give memory to getVisibleTodos

```javascript
import {createSelector} from 'reselect'

const getVisibleTodos = createSelector(
  [getVisibilityFilter, getTodos],
  (visibilityFilter, todos) => {
    switch (visibilityFilter) {
      case 'SHOW_ALL':
        return todos
      case 'SHOW_COMPLETED':
        return todos.filtler(t => t.completed)
      case 'SHOW_ACTIVE':
        return todos.filter(t => !t.completed)
    }
  }
)
```

And change our map to give the full state to it:

```javascript
const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state)
  }
}
```

And that's it! We've given memory to our function. No extra calculations if the involved parts of the state haven't changed!





> At Facebook, we use React in thousands of components, and we haven't found any use cases where we would recommend creating component inheritance hierarchies.

_Long live to composition, inheritance is dead._

So, how do you _extend_ a component in React?

Well, it's pretty obvious that the guys @Facebook consider inappropriate to inherit from parent components. Let's look at alternatives:

### Planning ahead of time

There might be some cases where you have a component which can't know what its children will be ahead of time (like most of us). For them, React gifts `props.children`:

```javascript
function List(props) {
  return (
    <ul className={'List List-' + props.importance}>
      {props.children}
    </ul>
    );
}

function DownloadMenu() {
  return (
    <List importance="High">
      <li>Download SBCL</li>
      <li>Download Racket</li>
      <li>Download Haskell</li>
    </List>
    );
}
```

As we can see, `props.children` receives everything that's in between the component's open and closing tag.

Furthermore, we can exploit `props` to fill voids:

```javascript
function ListWithHeader(props) {
  return (
    <ul className={'List List-' + props.importance}>
      <li className="List List-Header">
        {props.header}
      </li>
      {props.children}
    </ul>
    );
}

function DownloadMenuWithHeader() {
  return (
    <List importance="High" header={ <LispLogo /> }>
      <li>Download SBCL</li>
      ...
    </List>
    );
}
```

### Generic components and specialization

So, we've got this great _FolderView_ component

```javascript
function FolderView(props) {
  return (
    <div className="FolderView">
      <h1>{props.folderName}</h1>
      <ul className="FolderView FolderView-Actions">
        {props.availableActions}
      </ul>
      <ul className="FolderView FolderView-Files">
        {props.files}
      </ul>
    </div>
    );
}
```

This could represent any folder in our filesystem, however, we only want to _specialize_ it so it shows only the Pictures and Desktop folders.

```javascript
function DesktopFolder(props) {
  return (
    <FolderView folderName="Desktop"
      availableActions={
        <li>Create Folder</li>
        <li>Create Document</li>
      }
      files={props.files}
      />
    );
}

function PicturesFolder(props) {
  return (
    <FolderView
      folderName="Pictures"
      availableActions={
        <li>New Picture</li>
      }
      files={props.files}
      />
    );
}
```

And just like so, we've _specialized_ our component, without creating any inheritance hierarchy!





> I used to be an adventurer like you, but then I took a RTFM in the knee...
>
> Guard outside of Castle Reducer in the Land of React

So you've heard of this _React_ thing, and you actually peeked at the docs, maybe even went through some of the pages. Then you suddenly came across mysterious runes that spelled somewhat along the lines of Webpack, Browserify, Yarn, Babel, NPM and yet much more.

Thus, you went to their official sites and went through the docs, just to find yourself lost in the ever growing collection of modern tools (tm) to build web applications with JS.

Afraid you must be not.

### Yarn, NPM

They are just dependency managers, nothing to be afraid of. In fact, they are the less scary part. If you've ever used a GNU/Linux distro, FreeBSD, Homebrew on MacOSX, Ruby and Bundler, Lisp and Quicklisp just to name a few, you'll be familiar with the concept.

Should you not be, it's a very simple thing, so don't worry. The Land of React is but a province in a much bigger world, which some call Javascript and others Ecmascript (JS/ES for short).

Now suppose you are starting your own town, but you really want to use React's tools for your buildings, because, let's face it, it is popular. The traditional way would be to walk to the Land of React and ask the ruler to handle you some of his tools. The problem is you waste time, and you must keep contact with him so if any tools is faulty, you get a new one.

However, with the advent of drones and near-instant travel for them, some smart guys started a registry of the different towns, cities and provinces. Furthermore, they built especial drones that could deliver the tools right to you.

Nowadays, to require React in your project, you just have to go

```bash
$ npm init . # create town
$ npm install --save react # get react and put the materials in the store
```

Handy, isn't it? and free.

### Webpack, Browserify and friends

In the old times, you had to build your town all by yourself. If you wanted a statue of John McCarthy you would have to fetch marble and some parentheses on your own. This improved with the advent of _dependency managers_, which could fetch the marble and the parentheses for you. Yet, you had to assemble them on your own.

Then came some folks from Copy & Paste (inc) and developed some tools that allowed you to copy and paste your materials on some specific order, so you could have your parentheses over the statue or under it.

That was actually pretty cool, you just placed all the material and specified how they were going to be built, and all was peaceful for a while.

But this approach had a problem, which I'm sure other folks tried to circumvent. Namely, that if you wanted to replace the parentheses by cars you would have to rebuild your entire town. This was no problem for small towns, but large cities suffered by this approach.

Then inspiration was given by the mighty Lord and building and module bundlers were born.

This module bundlers allowed you to draw blueprints, and specify exactly how the town was to be constructed. Furthermore, they grew quickly and supported only rebuilding parts of the town, leaving the rest be.

### Babel

Babel is a time traveler who can bring materials from the future for you. like ES6/7/8 features.

### How they all mix together

Generally, you'll create a folder for your project, fetch dependencies, configure your module bundler so it knows where to search for your code and where to output the distributable. Furthermore, you may want to wire that to a development server so you can get instant feedback on what you're building.

However, the documentation of module bundlers is a bit overwhelming. There are so many choices and options for the novice adventurer that he might lose his motivation.

### create-react-app and friends

Thus yet another tool was created.

```bash
$ npm install -g create-react-app # globally install create-react-app
$ create-react-app my-shiny-new-app
```

And that's all. It comes pre-configured so you don't have to go through all of Webpack/Browserify docs just to test React. It also brings testing scripts, a development web server and much more.

However, this is not yet the final word to our history. There exists a myriad of different builders and tools for React, which can be seen here https://github.com/facebook/react/wiki/Complementary-Tools





React uses a mechanism called **reconciliation** for efficient rendering on update.

**Reconciliation** works by recursively comparing the tree of DOM elements, the specifics of the algorithm might be found on the official documents or the source, which is always the best source.

This mechanism is partly hidden, and as such, there are some conventions that might be followed to ease its inner workings.

One such example is **appending** vs **inserting**.

**Reconciliation** compares the list of root's nodes children at the same time, if two children differ, then React will mutate every one.

So, for example, if you've got the following:

```html
<ul>
    <li>Sherlock Holmes</li>
    <li>John Hamish Watson</li>
</ul>
```

And you insert an element at the beginning

```html
<ul>
    <li>Mycroft Holmes</li>
    <li>Sherlock Holmes</li>
    <li>John Hamish Watson</li>
</ul>
```

React will start of by comparing the old tree, where `<li>Second</li>` is the first child and the new tree where `<li>First</li>` is the new first child. Because they are different, it will mutate the children.

If, instead of inserting good Mycroft you appended him, React would perform better, not touching Sherlock nor John.

But you can't always append, sometimes you've just got to insert.

This is were **keys** come in. If you supply a **key** _attribute_ then React will figure out an efficient transformation from the old tree to the new one.

```html
<ul>
    <li key="3">Mycroft Holmes</li>
    <li key="1">Sherlock Holmes</li>
    <li key="2">John Hamish Watson</li>
</ul>
```

