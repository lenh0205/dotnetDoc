
# Power of React
* -> create reusable component and using JSX to combine it
* -> virtual DOM enables better interaction at less cost
* -> backed by a strong community: Facebook, developers around the world
* -> more plexible because it's library not a framework, so we can apply multiple kind of architect or library to complete our purpose
* -> Cross-platform Functionality

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
