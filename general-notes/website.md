# Website

[https://beta.reactjs.org/learn/add-react-to-a-website](https://beta.reactjs.org/learn/add-react-to-a-website)

React has been designed from the start for gradual adoption, and you can use as little or as much React as you need. Whether you’re working with micro-frontends, an existing system, or just giving React a try, you can start adding interactive React components to an HTML page with just a few lines of code—and no build tooling!

## Add React in one minute

You can add a React component to an existing HTML page in under a minute. Try this out with your own website or [an empty HTML file](https://www.notion.so/7b33305bf33776354797a2e3c1445186)—all you need is an internet connection and a text editor like Notepad (or VSCode—check out our guide on [how to set yours up](https://beta.reactjs.org/learn/editor-setup))!

### Step 1: Add an element to the HTML

In the HTML page you want to edit, add an HTML element like an empty `<div>` tag with a unique `id` to mark the spot where you want to display something with React.

You can place a “container” element like this `<div>` anywhere inside the `<body>` tag. React will replace any existing content inside HTML elements, so they are usually empty. You can have as many of these HTML elements on one page as you need.

### Step 2: Add the Script Tags

In the HTML page, right before the closing `</body>` tag, add three `<script>` tags for the following files:

```
  <script src="https://unpkg.com/react@17/umd/react.development.js" crossorigin></script>  <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js" crossorigin></script>  <script src="like_button.js"></script>
```

### Step 3: Create a React component

Create a file called **like\_button.js** next to your HTML page, add this code snippet, and save the file. This code defines a React component called `LikeButton`. [You can learn more about making components in our guides.](https://beta.reactjs.org/learn/your-first-component)

```
function LikeButton() {  const [liked, setLiked] = React.useState(false);
  if (liked) {
  return React.createElement(      onClick: () => setLiked(true),
```

### Step 4: Add your React Component to the page

Lastly, add two lines to the bottom of **like\_button.js**. These two lines of code find the `<div>` you added to your HTML in the first step and then display the “Like” button React component inside of it.

**Congratulations! You have just rendered your first React component to your website!**

### You can reuse components!

You might want to display a React component in multiple places on the same HTML page. This is most useful while React-powered parts of the page are isolated from each other. You can do this by calling `ReactDOM.render()` multiple times with multiple container elements.

1. In **index.html**, add an additional container element `<div id="component-goes-here-too"></div>`.
2. In **like\_button.js**, add an additional `ReactDOM.render()` for the new container element:

```
  React.createElement(LikeButton),  document.getElementById('component-goes-here')
  React.createElement(LikeButton),  document.getElementById('component-goes-here-too')
```

Check out [an example that displays the “Like” button three times and passes some data to it](https://www.notion.so/c0ea05cc33fbe75ad9bbf78e9044d7f8)!

### Step 5: Minify JavaScript for production

Unminified JavaScript can significantly slow down page load times for your users. Before deploying your website to production, it’s a good idea to minify its scripts.

* **If you don’t have a minification step** for your scripts, [here’s one way to set it up](https://www.notion.so/42a2ffa41b8319948f9be4076286e1f3).
* **If you already minify** your application scripts, your site will be production-ready if you ensure that the deployed HTML loads the versions of React ending in `production.min.js` like so:

## Try React with JSX

The examples above rely on features that are natively supported by browsers. This is why **like\_button.js** uses a JavaScript function call to tell React what to display:

However, React also offers an option to use [JSX](https://beta.reactjs.org/learn/writing-markup-with-jsx), an HTML-like JavaScript syntax, instead:

These two code snippets are equivalent. JSX is popular syntax for describing markup in JavaScript. Many people find it familiar and helpful for writing UI code—both with React and with other libraries. You might see “markup sprinkled throughout your JavaScript” in other projects!

The quickest way to try JSX in your project is to add the Babel compiler to your page’s `<head>` along with React and ReactDOM like so:

```
<script src="https://unpkg.com/react@17/umd/react.production.min.js" crossorigin></script><script src="https://unpkg.com/react-dom@17/umd/react-dom.production.min.js" crossorigin></script>
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
</head>
```

Now you can use JSX in any `<script>` tag by adding `type="text/babel"` attribute to it. For instance:

To convert **like\_button.js** to use JSX:

1. In **like\_button.js**, replace

```
return React.createElement(    onClick: () => setLiked(true),
```

with:

1. In **index.html**, add `type="text/babel"` to the like button’s script tag:

Here is [an example HTML file with JSX](https://raw.githubusercontent.com/reactjs/reactjs.org/main/static/html/single-file-example.html) that you can download and play with.

This approach is fine for learning and creating simple demos. However, it makes your website slow and **isn’t suitable for production**. When you’re ready to move forward, remove this new `<script>` tag and the `type="text/babel"` attributes you’ve added. Instead, in the next section you will set up a JSX preprocessor to convert all your `<script>` tags automatically.

### Add JSX to a project

Adding JSX to a project doesn’t require complicated tools like a [bundler](https://beta.reactjs.org/learn/start-a-new-react-project) or a development server. Adding a JSX preprocessor is a lot like adding a CSS preprocessor.

Go to your project folder in the terminal, and paste these two commands (**Be sure you have** [**Node.js**](https://nodejs.org) **installed!**):

1. `npm init -y` (if it fails, [here’s a fix](https://www.notion.so/246f6380610e262f8a648e3e51cad40d))
2. `npm install babel-cli@6 babel-preset-react-app@3`

You only need npm to install the JSX preprocessor. You won’t need it for anything else. Both React and the application code can stay as `<script>` tags with no changes.

Congratulations! You just added a **production-ready JSX setup** to your project.

### Run the JSX Preprocessor

You can preprocess JSX so that every time you save a file with JSX in it, the transform will be re-run, converting the JSX file into a new, plain JavaScript file.

1. Create a folder called **src**
2. In your terminal, run this command: `npx babel --watch src --out-dir . --presets react-app/prod` (Don’t wait for it to finish! This command starts an automated watcher for JSX.)
3. Move your JSX-ified **like\_button.js** to the new **src** folder (or create a **like\_button.js** containing this [JSX starter code](https://www.notion.so/ffbc9a0e33665a58d4cfdd1676f05453))

The watcher will create a preprocessed **like\_button.js** with the plain JavaScript code suitable for the browser.

As a bonus, this also lets you use modern JavaScript syntax features like classes without worrying about breaking older browsers. The tool we just used is called Babel, and you can learn more about it from [its documentation](https://babeljs.io/docs/en/babel-cli/).

If you’re getting comfortable with build tools and want them to do more for you, [we cover some of the most popular and approachable toolchains here](https://beta.reactjs.org/learn/start-a-new-react-project).
