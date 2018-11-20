
## Step 4: Load Bootstrap styles

We’re going to use Bootstrap styles to avoid reinventing the wheel.
We’re ignoring their JavaScript widgets and the amazing integration
built by the [react-bootstrap](http://react-bootstrap.github.io/) team.
Just the stylesheets please.

They’ll make our fonts look better, help with layouting, and make
buttons look like buttons. We *could* use styled components, but writing
our own styles detracts from this tutorial.

We load stylesheets in `src/index.js`.

    // src/index.js
    
    import React from 'react';
    import ReactDOM from 'react-dom';
    import App from './App';
    // markua-start-insert
    import 'bootstrap/dist/css/bootstrap.css';
    // markua-end-insert
    
    ReactDOM.render(
      <App />,
      document.getElementById('root')
    );

Another benefit of Webpack: `import`-ing stylesheets. These imports turn
into `<style>` tags with CSS in their body at runtime.

This is also a good opportunity to see how `index.js` works to render
our app 👇

1.  loads `App` and React
2.  loads styles
3.  Uses `ReactDOM` to render `<App />` into the DOM

That’s it.

Your preloader screen should look better now.

![Preloader
screenshot](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/preloader-screenshot.png)

If you don’t, try comparing your changes to this [diff on
Github](https://github.com/Swizec/react-d3js-step-by-step/commit/798ec9eca54333da63b91c66b93339565d6d582a).
