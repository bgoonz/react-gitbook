# Rendering Lists

[https://beta.reactjs.org/learn/rendering-lists](https://beta.reactjs.org/learn/rendering-lists)

You will often want to display multiple similar components from a collection of data. You can use the [JavaScript array methods](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global\_Objects/Array#) to manipulate an array of data. On this page, you’ll use `[filter()](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)` and `[map()](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/map)` with React to filter and transform your array of data into an array of components.

## Rendering data from arrays

Say that you have a list of content.

```
  <li>Creola Katherine Johnson: mathematician</li>  <li>Mario José Molina-Pasquel Henríquez: chemist</li>  <li>Mohammad Abdus Salam: physicist</li>  <li>Percy Lavon Julian: chemist</li>  <li>Subrahmanyan Chandrasekhar: astrophysicist</li>
```

The only difference among those list items are their contents, their data. You will run into many situations where you need many of the same component using different data when building interfaces: from lists of comments to galleries of profile images. In these situations, you can store that data in JavaScript objects and arrays and use methods like `[map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)` and `[filter()](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)` to render lists of components from them.

Here’s a short example of how to generate a list of items from an array:

1. **Move** the data into an array:

```
const people = [
```

1. **Map** the `people` members into a new array of JSX nodes, `listItems`:
2. **Return** `listItems` from your component wrapped in a `<ul>`:

Here is the result:

const people = \[

export default function List() {

const listItems = people.map(person =>

* {person}

return

* {listItems}

;

## Filtering arrays of items

This data can be structured even more.

```
const people = [{  id: 0,  name: 'Creola Katherine Johnson',  profession: 'mathematician',}, {  id: 1,  name: 'Mario José Molina-Pasquel Henríquez',  profession: 'chemist',}, {  id: 2,  name: 'Mohammad Abdus Salam',  profession: 'physicist',}, {  name: 'Percy Lavon Julian',  profession: 'chemist',  }, {  name: 'Subrahmanyan Chandrasekhar',  profession: 'astrophysicist',
```

Let’s say you want a way to only show people whose profession is `'chemist'`. You can use JavaScript’s `filter()` method to return just those people. This method takes an array of items, passes them through a “test” (a function that returns `true` or `false`), and returns a new array of only those items that passed the test (returned `true`).

You only want the items where `profession` is `'chemist'`. The “test” function for this looks like `(person) => person.profession === 'chemist'`. Here’s how to put it together:

1. **Create** a new array of just “chemist” people, `chemists`, by calling `filter()` on the `people` filtering by `person.profession === 'chemist'`:
2. Now **map** over `chemists`:

```
const listItems = chemists.map(person =>       src={getImageUrl(person)}       alt={person.name}       <b>{person.name}:</b>       {' ' + person.profession + ' '}       known for {person.accomplishment}
```

1. Lastly, **return** the `listItems` from your component:

import { people } from './data.js';

import { getImageUrl } from './utils.js';

export default function List() {

const chemists = people.filter(person =>

person.profession === 'chemist'

const listItems = chemists.map(person =>

src={getImageUrl(person)}

alt={person.name}

**{person.name}:**

{' ' + person.profession + ' '}

known for {person.accomplishment}

return

* {listItems}

;

Arrow functions implicitly return the expression right after `=>`, so you didn’t need a `return` statement:

However, **you must write `return` explicitly if your `=>` is followed by a `{` curly brace!**

Arrow functions containing `=> {` are said to have a [“block body”](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow\_functions#function\_body). They let you write more than a single line of code, but you _have to_ write a `return` statement yourself. If you forget it, nothing gets returned!

## Keeping list items in order with `key`

If you open any of the sandboxes above in a new tab, you’ll see an error in the console:

You need to give each array item a `key` — a string or a number that uniquely identifies it among other items in that array:

Keys tell React which array item each component corresponds to, so that it can match them up later. This becomes important if your array items can move (e.g. due to sorting), get inserted, or get deleted. A well-chosen `key` helps React infer what exactly has happened, and make the correct updates to the DOM tree.

Rather than generating keys on the fly, you should include them in your data:

export const people = \[{

id: 0, // Used in JSX as a key

name: 'Creola Katherine Johnson',

profession: 'mathematician',

accomplishment: 'spaceflight calculations',

}, {

id: 1, // Used in JSX as a key

name: 'Mario José Molina-Pasquel Henríquez',

profession: 'chemist',

accomplishment: 'discovery of Arctic ozone hole',

}, {

id: 2, // Used in JSX as a key

name: 'Mohammad Abdus Salam',

profession: 'physicist',

accomplishment: 'electromagnetism theory',

}, {

id: 3, // Used in JSX as a key

name: 'Percy Lavon Julian',

profession: 'chemist',

accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',

}, {

id: 4, // Used in JSX as a key

name: 'Subrahmanyan Chandrasekhar',

profession: 'astrophysicist',

accomplishment: 'white dwarf star mass calculations',

}];

### Where to get your `key`

Different sources of data provide different sources of keys:

* **Data from a database:** If your data is coming from a database, you can use the database keys/IDs, which are unique by nature.
* **Locally generated data:** If your data is generated and persisted locally (e.g. notes in a note-taking app), use an incrementing counter or a package like `[uuid](https://www.npmjs.com/package/uuid)` when creating items.
* **Keys must be unique among siblings.** However, it’s okay to use the same keys for JSX nodes in _different_ arrays.
* **Keys must not change** or that defeats their purpose! Don’t generate them while rendering.

### Why does React need keys?

Imagine that files on your desktop didn’t have names. Instead, you’d refer to them by their order — the first file, the second file, and so on. You could get used to it, but once you delete a file, it would get confusing. The second file would become the first file, the third file would be the second file, and so on.

File names in a folder and JSX keys in an array serve a similar purpose. They let us uniquely identify an item between its siblings. A well-chosen key provides more information than the position within the array. Even if the _position_ changes due to reordering, the `key` lets React identify the item througout its lifetime.

### Gotcha

You might be tempted to use an item’s index in the array as its key. In fact, that’s what React will use if you don’t specify a `key` at all. But the order in which you render items will change over time if an item is inserted, deleted, or if the array gets reordered. Index as a key often leads to subtle and confusing bugs.

Similarly, do not generate keys on the fly, e.g. with `key={Math.random()}`. This will cause keys to never match up between renders, leading to all your components and DOM being recreated every time. Not only is this slow, but it will also lose any user input inside the list items. Instead, use a stable ID based on the data.

Note that your components won’t receive `key` as a prop. It’s only used as a hint by React itself. If your component needs an ID, you have to pass it as a separate prop: `<Profile key={id} userId={id} />`.

## Recap

On this page you learned:

* How to move data out of components and into data structures like arrays and objects.
* How to generate sets of similar components with JavaScript’s `map()`.
* How to create arrays of filtered items with JavaScript’s `filter()`.
* Why and how to set `key` on each component in a collection so React can keep track of each of them even if their position or data changes.

This example shows a list of all people.

Change it to show two separate lists one after another: **Chemists** and **Everyone Else**. Like previously, you can determine whether a person is a chemist by checking if `person.profession === 'chemist'`.

import { people } from './data.js';

import { getImageUrl } from './utils.js';

export default function List() {

const listItems = people.map(person =>

*   src={getImageUrl(person)}

    alt={person.name}

    **{person.name}:**

    {' ' + person.profession + ' '}

    known for {person.accomplishment}

    ## Scientists

    * {listItems}
