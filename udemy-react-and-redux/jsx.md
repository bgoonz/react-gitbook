# JSX

## JSX (Section 1,2)

React project directory structure:

![](file:///C:/Users/bryan/AppData/Local/Temp/msohtmlclip1/01/clip\_image002.png)

Start and stop application:

![](file:///C:/Users/bryan/AppData/Local/Temp/msohtmlclip1/01/clip\_image004.png)

| **Starting file:** | `src/index.js` |
| ------------------ | -------------- |

Three components inside `src/index.js` file:

1\. Import React and ReactDom libraries

2\. Create a React component

3\. Take the react component and show it on the screen

| <pre><code>import React from "react";</code></pre><pre><code>import ReactDOM from "react-dom";</code></pre><pre><code> </code></pre><pre><code>const App = () => {</code></pre><pre><code>  const buttonText = "Click Me!";</code></pre><pre><code> </code></pre><pre><code>  return (</code></pre><pre><code>    &#x3C;div></code></pre><pre><code>      &#x3C;label className="label" htmlFor="name">Enter name:&#x3C;/label></code></pre><pre><code>      &#x3C;input id="name" type="text" /></code></pre><pre><code>      &#x3C;button style={{ backgroundColor: "blue", color: "white" }}></code></pre><pre><code>        {buttonText}</code></pre><pre><code>      &#x3C;/button></code></pre><pre><code>    &#x3C;/div></code></pre><pre><code>  );</code></pre><pre><code>};</code></pre><pre><code> </code></pre><pre><code>ReactDOM.render(&#x3C;App />, document.querySelector("#root"));</code></pre> | ![](file:///C:/Users/bryan/AppData/Local/Temp/msohtmlclip1/01/clip\_image005.png) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |

Note:

`document.querySelector("#root")` refers to element with id of “root” in file:

| **public/index.html**                                                                                                      |
| -------------------------------------------------------------------------------------------------------------------------- |
| <pre><code>&#x3C;body>  </code></pre><pre><code>  &#x3C;div id="root">&#x3C;/div> </code></pre><pre><code>  …</code></pre> |

### Module Systems <a href="#jsx-section1-2-modulesystems" id="jsx-section1-2-modulesystems"></a>

![](file:///C:/Users/bryan/AppData/Local/Temp/msohtmlclip1/01/clip\_image006.png)

### What is a React Component

![](file:///C:/Users/bryan/AppData/Local/Temp/msohtmlclip1/01/clip\_image008.png)

### What is JSX

![](file:///C:/Users/bryan/AppData/Local/Temp/msohtmlclip1/01/clip\_image010.png)

![](file:///C:/Users/bryan/AppData/Local/Temp/msohtmlclip1/01/clip\_image012.png)

![](file:///C:/Users/bryan/AppData/Local/Temp/msohtmlclip1/01/clip\_image014.png)
