# Node.js versus Next.js - A React Approach

Back in the early days when websites were becoming interactive, JavaScript was introduced. The JS interpreter was present in the browser and did an amazing job of making websites interactive. All the computations were done on the local machine using in the in-built JS Engine. The makers of JS took JS which is confined to the browser and made it available to the local machine. With this change, a lot of new features were unlocked.

* Access Local files
* Monitor Networks
* Listen to HTTP requests
* Access Database directly

There are two types of applications built with Node.js. The first one are utility applications which are used to enhance the performance of the main application. For example, hot-reload functionality is a utility function. While coding, the changes are made as we code along. The second type of applications built with Node.js are web servers that handle requests from other sites to provide software services.

#### Next.js <a href="#nextjs" id="nextjs"></a>

The official website talks about why Next.js was introduced. Let us discuss the reasons in detail to get a perspective about Next.js.

* In his [blog](https://rauchg.com/2014/7-principles-of-rich-web-applications), Guillermo Rauch talks about the seven principles for creating rich web applications. In short, they are

1. Pre-rendering pages for optimal performance
2. User-Input must be dealt with immediately: For example, when we type into the Google search box, results are displayed instantly without any reloading. AJAX, which stands Asynchronous JavaScript And XML, is an example of this.
3. Let the user know about changes in data on the server without asking. This ensures minimal reloading and maximum data throughout the screen.
4. Ensuring proper data exchange with the server: Libraries like Axios, Node-fetch, etc. have enhanced the level of control we have over network calls. Therefore, taking full advantage of the network calls to maximize user experience is necessary. Error handling, data synchronization, and offline caches are essential for optimal levels of data exchange with the server irrespective of network conditions.
5. Consider the problem of infinite pagination. Since web browsers don’t store history, the problem of navigation is a significant one. User’s don’t expect changes when visiting the previous pages. Therefore maintaining custom local caches is essential for fast feedback.
6. Push code updates: Data handling and API calls must be handled carefully. Changes in data should correspond to changes in code as well. This is predominantly done to avoid errors during data fetch.
7. Predict user’s behavior: Understand patterns and have mechanisms for predicting the final user input.

* Building a PHP like system with the convenience and vast resources of Node.js.
* Enabling easy-to-setup server-side rendering for React. Node.js does offer support for server-side rendering, but the setup is tedious and involves a steep learning curve.

#### Server Side Rendering vs. Client-Side Rendering <a href="#server-side-rendering-vs-client-side-rendering" id="server-side-rendering-vs-client-side-rendering"></a>

Walmart Labs has written a well detailed [blog](https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8) on the subject of performance enhancement using server-side rendering. Make sure to check that out. It details the performance enhancement that Next.js enables concerning the create-react-app package.

The two diagrams mentioned below detail the processes involved in server-side rendering and client-side rendering. We observe that server-side rendered applications are one step ahead of client-side rendered applications. They display the HTML and then load the JS files, therefore enhancing the user experience in terms of performance and loading time. It also falls in line with the 7 principles listed above.

![](https://www.section.io/engineering-education/node-versus-next-react-approach/ssr.jpg)

[Image Source](https://www.section.io/engineering-education/node-versus-next-react-approach/medium.com)

![](https://www.section.io/engineering-education/node-versus-next-react-approach/csr.jpg)

[Image Source](https://www.section.io/engineering-education/node-versus-next-react-approach/medium.com)

We have looked at building React applications using create-react-app in a previous two article series. You can review them [here](https://www.section.io/build-an-outlook-clone-using-react/) and [here](https://www.section.io/build-an-outlook-clone-using-react-hooks/).

We will be using `create-next-app` instead of `create-react-app` for building next.js applications. Next.js [documentation](https://nextjs.org/learn/basics/create-nextjs-app) provides a detailed step-by-step tutorial on how to implement a Next.js project. We will focus on the major differences between the two and understand the features in depth.

Node.js provides a very vast framework and includes many libraries to support every need a developer might come across. Next.js enhances the user experience by introducing server-side rendering with a lower threshold for developers. Using the existing React knowledge, developers can build SSR applications.

#### Node.js vs. Next.js <a href="#nodejs-vs-nextjs" id="nodejs-vs-nextjs"></a>

1. We use `create-react-app` for building React applications with Node.js. On the contrary, `create-next-app`(CNA) is used for building applications with Next.js.
2. Discussing the differences between the frameworks should begin with the directory structures. `create-react-app`(CRA) generates the following directory structure.

```
nodesampleproject
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    └── serviceWorker.js
```

whereas, `create-next-app` generates the directory structure below:

```
nextsampleproject
├── README.md
├── .next
├── node_modules
├── package.json
├── .gitignore
├── styles
│   ├── globals.css
│   ├── Home.module.css
├── public
│   ├── favicon.ico
│   ├── vercel.svg
└── pages
    ├── api
        ├── hello.js
    ├── _app.js_
    ├── index.js
```

1. The App component in CRA is the main file called _Index.js_. _App.js_ is the parent component that calls all the child components. On the contrary, in _Next.js_, the _\_app.js_ file enables loading initial props using getInitialProps function. This function offers server-side rendering by preloading the webpage with the initial props.
2. In `create-react-app`, we need to run `npm run build` to make the code production-ready. Next.js offers many functionalities including webpack compiling and code splitting. We can specify our webpack and the babel configs in package.json as well.
3. Next.js offers `react-fast-refresh`, which is an enhanced version of React Hot Reloading provided by CRA. It makes the react app reloads very smooth and fast with changes in code. A react-native concept has been included in the Next.js framework.
4. Next.js offers static file serving. This way all resources like images, CSS files, robots.txt, and global js files can be placed in the public folder, and such resources can be easily called using relative paths. `create-react-app` does allow static file serving. When it comes to JS files they need to be structured as an external library. Furthermore, they are stored in a global variable referenced within the code.
5. Next.js is ready for production from the start of the project. Using `create-react-app`, the deployment process is lengthy. Depending on the type of website, the deployment techniques differ. These can be found in the [documentation](https://create-react-app.dev/docs/deployment/) for `create-react-app`
6. HTML files received when loading an application built using Next.js are shown in the figure below. The website you are viewing this blog on, is client-side rendered. Let’s do something fun. Press **Cltrl+Shift+I** to access the developer tools and click on the **elements** option. You will find a _short piece of HTML_ calling JS files as and when user inputs are received. Review the differences between the HTML file that you observe in the console and the image shown below.

![](https://www.section.io/engineering-education/node-versus-next-react-approach/ssr\_lookup.jpg) _Screenshot of the HTML rendered on Server-Side._

1. SSR enables enhanced Search-Engine-Optimization. SEO is easily performed using Next because search engines have more data to parse through. This helps websites stand out within search engines.

#### Conclusion <a href="#conclusion" id="conclusion"></a>

We have gone through a huge list of differences between Next.js and create-react-app. Although Next.js beats create-react-app applications in most of the cases, create-react-app is the most suitable in cases of CSR Applications. CSR applications might be a necessity when SSR is not an option, for example, when videos are played. Companies like Netflix take advantages of both worlds and designed superior systems. We hope you keep in mind the differences and points mentioned above while designing your applications. Be legendary.
