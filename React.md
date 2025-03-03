===========================================================================
# Power of React
* -> **`component-based`** and **`JSX`**: write UI using Javascript code in XML style; resuable and self-contained component
* -> **`Virtual DOM`** enables better interaction at less cost
* -> **`Unidirectional Data Flow`** - data flows from parent components to child components so it's more predictable and easier to debug
* -> **`Hooks`** - interfere in lifecycle and manage state better
* -> support **`Server-side rendering`** - React can be used for server-side rendering, which can improve the initial loading time and SEO performance of applications
* -> strong **`community`**: Facebook, developers around the world

# Different between 'client-side rendering' and 'server-side rendering' ?
## server-side rendering
* -> is the process of rendering web pages on the server and sending the fully-rendered HTML to the client

* _Advantages:_
* -> Faster initial load times
* -> Improved SEO optimization
* -> Can provide a better user experience for users with slower internet connections or less powerful devices

* _Disadvantages:_
* -> can require more server resources and maintenance
* -> this can result in slower subsequent page loads if the client needs to make additional server requests

## client-side rendering
* -> is the process of rendering web pages on the client using JavaScript

* _Advantages:_
* -> More dynamic and interactive web applications
* -> Can provide a smoother and more seamless user experience
* -> Can reduce the need for additional server requests

* _Disadvantages:_
* -> Slower initial load times
* -> Can be less SEO-friendly, as search engines may have difficulty indexing client-rendered content

## Server-Side Generation 
* -> Server-side generation (SSG) is a hybrid approach that combines the benefits of SSR and CSR
* -> In this approach, the server generates static HTML files for each page, but also includes client-side JavaScript that can be used to update the page as needed.
* -> a popular SSG framework is **Gatsby**

===========================================================================
# Typescript
* -> 

===========================================================================
# HTML5 and Features
* -> HTML5 introduced semantic elements (<article>, <section>), multimedia support (<audio>, <video>), form enhancements, offline storage (localStorage, sessionStorage), and better APIs (Canvas, WebSockets, Geolocation).

# HTML Semantics
* -> Semantic tags (<header>, <nav>, <main>, etc.) improve accessibility, SEO, and code readability. Search engines understand content better, and screen readers provide a better user experience.

# Async vs. Defer
* -> async: Loads and executes the script immediately, without waiting for the DOM to load
* -> defer: Waits for the DOM to be fully parsed before executing
* => Use async for independent scripts (does not depend on DOM); for example, third-party scripts like analytics
* => defer for scripts that depend on the DOM

```html
<!-- Script without async or defer (blocks page rendering) -->
<script src="script.js"></script>

<!-- Async: Loads script while parsing HTML but executes as soon as it's ready -->
<script async src="script.js"></script>

<!-- Defer: Loads script while parsing HTML and executes after DOM is fully parsed -->
<script defer src="script.js"></script>
```

# <img> vs. <picture>
* -> <img> loads a single image, while <picture> allows multiple sources for responsive images, improving performance and adaptability.

# Important <form> Attributes & Security
* -> action, method, enctype, novalidate
* -> Security: Use HTTPS, validate inputs, implement CSRF protection, and sanitize user input

# <div> vs. <span> vs. HTML5 Sections
* -> <div>: Block-level container
* -> <span>: Inline container
* -> <section>, <article>: Semantic content grouping for SEO and accessibility.

# data-* Attributes
* -> Custom attributes for storing extra data. Example: <button data-user-id="123">Click</button>, accessible via JavaScript (element.dataset.userId).

# SEO-friendly URL & Tags
* -> Clean URLs (/blog/how-to-code instead of /blog?id=123)
* -> Use <title>, <meta description>, <h1>, <alt> attributes, and structured data.

```bash
$ https://example.com/article?id=123 # not SEO-friendly
$ https://example.com/article/how-to-code # SEO-Friendly
```
 
# onload vs. DOMContentLoaded vs. onreadystatechange
* -> onload: Fires when everything (images, styles) is loaded.
* -> DOMContentLoaded: Fires when HTML is parsed but before resources are loaded.
* -> onreadystatechange: Fires at different loading stages.

# Page Load Optimization
* -> Minimize HTML, CSS, JS
* -> Use async/defer for scripts
* -> Optimize images, enable compression (Gzip, Brotli)
* -> Use lazy loading (loading="lazy" for images)

# ARIA (Accessible Rich Internet Applications)
* -> Attributes (aria-label, aria-hidden, role) help screen readers and improve accessibility.

# Avoiding <table> for Layout
* -> Tables are rigid, not responsive, and harm accessibility. Use CSS Flexbox/Grid instead.

# Embedding HTML Documents
* -> Use <iframe src="page.html"></iframe> to embed another HTML document.

# Embedding Video & Audio
* -> <video> with controls, autoplay, loop, poster
* -> <audio> with controls, autoplay, loop

# Cookies vs. localStorage vs. sessionStorage
* -> Cookies: Small, sent with every request, expires, used for authentication
* -> localStorage: Persistent storage, no expiration
* -> sessionStorage: Temporary storage, cleared when the session ends

# Shadow DOM
* -> Encapsulates styles and scripts within a component, used in Web Components.

# Microdata & SEO
* -> Structured data (schema.org) helps search engines understand content. Example:

```html
<div itemscope itemtype="https://schema.org/Article">
  <h1 itemprop="headline">SEO Guide</h1>
</div>
```

# Handling CORS
* -> Configure server CORS headers
* -> Use a proxy
* -> Use JSONP (legacy)

# WebSockets in HTML
* -> Real-time communication using JavaScript:

```js
const ws = new WebSocket("ws://server.com");
ws.onmessage = (event) => console.log(event.data);
```

# <link> & <meta> for SEO & Performance
* -> <link>: External resources (stylesheet, preload, dns-prefetch).
* -> <meta>: Page metadata, charset, viewport, description, robots.

# CSS Box Model
* -> Elements consist of content, padding, border, margin.

```css
/* common misunderstanding */
/* The actual width = 200px + 20px + 10px + 20px + 10px = 260px (not 200px) */
.box {
  width: 200px;
  padding: 20px;
  border: 10px solid black;
}

/* Fix using box-sizing: border-box */
.box {
  box-sizing: border-box; /* Includes padding & border in width */
}
```

# CSS Units (%, px, em, rem)
* -> %: Relative to parent
* -> px: Fixed
* -> em: Relative to parent font size
* -> rem: Relative to root font size

# Flexbox
* -> justify-content: Aligns items horizontally
* -> align-items: Aligns items vertically
* -> flex-direction: Defines row/column layout
* -> flex-wrap: Wrap items if needed

# Flexbox vs. Grid
* -> Flexbox: One-dimensional layout (row/column).
* -> Grid: Two-dimensional layout (rows & columns)

```css
/* if the container has no column definition, this will not work */
.container {
  display: grid;
}
.item {
  grid-column: 1 / 3; /* Spans 2 columns */
}

/* Fixed: */
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
```

# Pseudo-classes vs. Pseudo-elements
* -> :hover, :nth-child(): Pseudo-classes (modify states)
* -> ::before, ::after: Pseudo-elements (insert content)

# z-index & Stacking Context
* -> Controls layering; a higher z-index appears above lower ones.

# SASS & LESS
* -> CSS preprocessors. SASS is more popular, LESS is simpler.

# CSS Variables
```cs
:root { --main-color: blue; }
div { color: var(--main-color); }
```

# Responsive Design Methods
* -> Media queries
* -> Flexbox/Grid
* -> Viewport units (vh, vw)
* -> min-width, max-width

# CSS Specificity
* -> More specific rules override others (id > class > element).

# CSS-in-JS (Styled Components, Emotion)
* -> Writing CSS in JavaScript for component-based styling.

# Reset vs. Normalize CSS
* -> Reset: Removes all default styles.
* -> Normalize: Standardizes styles across browsers.

# CSS Optimizations
* -> Minification, critical CSS, avoiding unused styles.

# Basic Data Types
* -> String, Number, Boolean, Object, Array, undefined, null, Symbol, BigInt.

# == vs. ===
* -> ==: Loose comparison (converts types)
* -> ===: Strict comparison (checks both type & value)

# let, const, var
* -> var: Function-scoped, hoisted
* -> let: Block-scoped
* -> const: Block-scoped, cannot be reassigned

# Closures
* -> Functions that remember variables from their outer scope.

```js
function outer() { let count = 0; return () => count++; }
```

# Hoisting
* -> Variables and functions are moved to the top of their scope

```js
console.log(x); // ❌ ReferenceError
let x = 5;

console.log(y); // ✅ undefined (hoisted)
var y = 5;
```

# Arrow Functions vs. Regular Functions
* -> No this binding in arrow functions.
* -> Cannot use arguments.

# Promises & Async/Await
* -> Handle async code without callbacks.

```js
async function fetchData() { let data = await fetch(url); }
```

# Event Loop
* -> Call Stack, Task Queue, Microtask Queue (Prometheus > Callbacks)

# .map(), .filter(), .reduce()
* -> .map(): Transforms an array
* -> .filter(): Filters items
* -> .reduce(): Accumulates values

# Prototype & Prototype Chain
* -> Inheritance mechanism in JavaScript.

# Debounce vs. Throttle
* -> Debounce: Delay function execution (_useful for search box_)
* -> Throttle: Limit function execution rate (_useful for scroll events_)

```js
function searchAPI(query) {
  console.log("Fetching results for:", query);
}

const debounce = (func, delay) => {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => func(...args), delay);
  };
};

const search = debounce(searchAPI, 500);
search("JavaScript"); // Will execute after 500ms
```

# Virtual DOM in React
* -> Optimized re-rendering using a diffing algorithm.

# Event Delegation
* -> Attach event listener to a parent instead of multiple children.
