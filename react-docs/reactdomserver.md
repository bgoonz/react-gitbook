# ReactDOMServer

The `ReactDOMServer` object enables you to render components to static markup. Typically, it's used on a Node server:

```js
// ES modules
import ReactDOMServer from 'react-dom/server';
// CommonJS
var ReactDOMServer = require('react-dom/server');
```

### Overview <a href="#overview" id="overview"></a>

The following methods can be used in both the server and browser environments:

* [`renderToString()`](broken-reference)
* [`renderToStaticMarkup()`](broken-reference)

These additional methods depend on a package (`stream`) that is **only available on the server**, and won't work in the browser.

* [`renderToNodeStream()`](broken-reference)
* [`renderToStaticNodeStream()`](broken-reference)

***

### Reference <a href="#reference" id="reference"></a>

#### `renderToString()` <a href="#rendertostring" id="rendertostring"></a>

```javascript
ReactDOMServer.renderToString(element)
```

Render a React element to its initial HTML. React will return an HTML string. You can use this method to generate HTML on the server and send the markup down on the initial request for faster page loads and to allow search engines to crawl your pages for SEO purposes.

If you call `ReactDOM.hydrate()` on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.

***

#### `renderToStaticMarkup()` <a href="#rendertostaticmarkup" id="rendertostaticmarkup"></a>

```javascript
ReactDOMServer.renderToStaticMarkup(element)
```

Similar to [`renderToString`](broken-reference), except this doesn't create extra DOM attributes that React uses internally, such as `data-reactroot`. This is useful if you want to use React as a simple static page generator, as stripping away the extra attributes can save some bytes.

If you plan to use React on the client to make the markup interactive, do not use this method. Instead, use [`renderToString`](broken-reference) on the server and `ReactDOM.hydrate()` on the client.

***

#### `renderToNodeStream()` <a href="#rendertonodestream" id="rendertonodestream"></a>

```javascript
ReactDOMServer.renderToNodeStream(element)
```

Render a React element to its initial HTML. Returns a [Readable stream](https://nodejs.org/api/stream.html#stream\_readable\_streams) that outputs an HTML string. The HTML output by this stream is exactly equal to what [`ReactDOMServer.renderToString`](broken-reference) would return. You can use this method to generate HTML on the server and send the markup down on the initial request for faster page loads and to allow search engines to crawl your pages for SEO purposes.

If you call `ReactDOM.hydrate()` on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.

> Note:
>
> Server-only. This API is not available in the browser.
>
> The stream returned from this method will return a byte stream encoded in utf-8. If you need a stream in another encoding, take a look at a project like [iconv-lite](https://www.npmjs.com/package/iconv-lite), which provides transform streams for transcoding text.

***

#### `renderToStaticNodeStream()` <a href="#rendertostaticnodestream" id="rendertostaticnodestream"></a>

```javascript
ReactDOMServer.renderToStaticNodeStream(element)
```

Similar to [`renderToNodeStream`](broken-reference), except this doesn't create extra DOM attributes that React uses internally, such as `data-reactroot`. This is useful if you want to use React as a simple static page generator, as stripping away the extra attributes can save some bytes.

The HTML output by this stream is exactly equal to what [`ReactDOMServer.renderToStaticMarkup`](broken-reference) would return.

If you plan to use React on the client to make the markup interactive, do not use this method. Instead, use [`renderToNodeStream`](broken-reference) on the server and `ReactDOM.hydrate()` on the client.

> Note:
>
> Server-only. This API is not available in the browser.
>
> The stream returned from this method will return a byte stream encoded in utf-8. If you need a stream in another encoding, take a look at a project like [iconv-lite](https://www.npmjs.com/package/iconv-lite), which provides transform streams for transcoding text.
