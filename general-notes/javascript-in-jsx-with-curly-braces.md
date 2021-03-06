# JavaScript in JSX with Curly Braces

## Copy of JavaScript in JSX with Curly Braces

[https://beta.reactjs.org/learn/javascript-in-jsx-with-curly-braces](https://beta.reactjs.org/learn/javascript-in-jsx-with-curly-braces)

JSX lets you write HTML-like markup inside a JavaScript file, keeping rendering logic and content in the same place. Sometimes you will want to add a little JavaScript logic or reference a dynamic property inside that markup. In this situation, you can use curly braces in your JSX to open a window to JavaScript.

#### You will learn

* How to pass strings with quotes
* How to reference a JavaScript variable inside JSX with curly braces
* How to call a JavaScript function inside JSX with curly braces
* How to use a JavaScript object inside JSX with curly braces

### Passing strings with quotes

When you want to pass a string attribute to JSX, you put it in single or double quotes:

export default function Avatar() {

Here, `"https://i.imgur.com/7vQD0fPs.jpg"` and `"Gregorio Y. Zara"` are being passed as strings.

But what if you want to dynamically specify the `src` or `alt` text? You could **use a value from JavaScript by replacing `"` and `"` with `{` and `}`**:

export default function Avatar() {

const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';

const description = 'Gregorio Y. Zara';

src={avatar}

alt={description}

}

Notice the difference between `className="avatar"`, which specifies an `"avatar"` CSS class name that makes the image round, and `src={avatar}` that reads the value of the JavaScript variable called `avatar`. That’s because curly braces let you work with JavaScript right there in your markup!

### Using curly braces: A window into the JavaScript world

JSX is a special way of writing JavaScript. That means it’s possible to use JavaScript inside it—with curly braces `{ }`. The example below first declares a name for the scientist, `name`, then embeds it with curly braces inside the `<h1>`:

Try changing `name`’s value from `'Gregorio Y. Zara'` to `'Hedy Lamarr'`. See how the To Do List title changes?

Any JavaScript expression will work between curly braces, including function calls like `formatDate()`:

const today = new Date();

function formatDate(date) {

return new Intl.DateTimeFormat(

{ weekday: 'long' }

).format(date);

}

export default function TodoList() {

## To Do List for {formatDate(today)}

You can only use curly braces in two ways inside JSX:

1. **As text** directly inside a JSX tag: `<h1>{name}'s To Do List</h1>` works, but `<{tag}>Gregorio Y. Zara's To Do List</{tag}>` will not.
2. **As attributes** immediately following the `=` sign: `src={avatar}` will read the `avatar` variable, but `src="{avatar}"` will pass the string `{avatar}`.

### Using “double curlies”: CSS and other objects in JSX

In addition to strings, numbers, and other JavaScript expressions, you can even pass objects in JSX. Objects are also denoted with curly braces, like `{ name: "Hedy Lamarr", inventions: 5 }`. Therefore, to pass a JS object in JSX, you must wrap the object in another pair of curly braces: `person={{ name: "Hedy Lamarr", inventions: 5 }}`.

You may see this with inline CSS styles in JSX. React does not require you to use inline styles (CSS classes work great for most cases). But when you need an inline style, you pass an object to the `style` attribute:

export default function TodoList() {

backgroundColor: 'black',

\}}>

* Improve the videophone
* Prepare aeronautics lectures
* Work on the alcohol-fuelled engine

}

Try changing the values of `backgroundColor` and `color`.

You can really see the JavaScript object inside the curly braces when you write it like this:

The next time you see `{{` and `}}` in JSX, know that it’s nothing more than an object inside the JSX curlies!

### More fun with JavaScript objects and curly braces

You can move several expressions into one object, and reference them in your JSX inside curly braces:

const person = {

name: 'Gregorio Y. Zara',

theme: {

backgroundColor: 'black',

export default function TodoList() {

## {person.name}'s Todos

* Improve the videophone
* Prepare aeronautics lectures
* Work on the alcohol-fuelled engine

In this example, the `person` JavaScript object contains a `name` string and a `theme` object:

```
  name: 'Gregorio Y. Zara',
```

The component can use these values from `person` like so:

JSX is very minimal as a templating language because it lets you organize data and logic using JavaScript.

### Recap

Now you know almost everything about JSX:

* JSX attributes inside quotes are passed as strings.
* Curly braces let you bring JavaScript logic and variables into your markup.
* They work inside the JSX tag content or immediately after `=` in attributes.
* `{{` and `}}` is not special syntax: it’s a JavaScript object tucked inside JSX curly braces.

This code crashes with an error saying `Objects are not valid as a React child`:

name: 'Gregorio Y. Zara',

backgroundColor: 'black',

export default function TodoList() {

## {person}'s Todos

* Improve the videophone
* Prepare aeronautics lectures
* Work on the alcohol-fuelled engine

Can you find the problem?
