
We `import` React (which we need to make JSX syntax work) and the
`PreloaderImg` for our image. We can import images because of the
Webpack configuration that comes with `create-react-app`. The webpack
image loader returns a URL that we put in the `PreloaderImg` constant.

At the bottom, we `export default Preloader` so that we can use it in
`App.js` as `import Preloader`. Default exports are great when your file
exports a single object. Named exports when you want to export multiple
items. You’ll see that play out in the rest of this project.

The `Preloader` function takes no props (because we don’t need any) and
returns an empty `div`. Let’s fill it in.

    // src/components/Preloader.js
    
    const Preloader = () => (
        <div className="App container">
            <h1>The average H1B in tech pays $86,164/year</h1>
            <p className="lead">
                Since 2012 the US tech industry has sponsored 176,075 H1B work
                visas. Most of them paid <b>$60,660 to $111,668</b> per year (1
                standard deviation).{" "}
                <span>
                    The best city for an H1B is <b>Kirkland, WA</b> with an average
                    individual salary <b>$39,465 above local household median</b>.
                    Median household salary is a good proxy for cost of living in an
                    area.
                </span>
            </p>
            <img
                src={PreloaderImg}
                style={{ width: "100%" }}
                alt="Loading preview"
            />
            <h2 className="text-center">Loading data ...</h2>
        </div>
    );

A little cheating with grabbing copy from the future, but that’s okay.
In real life you’d use some temporary text, then fill it in later.

The code itself looks like HTML. We have the usual tags - `h1`, `p`,
`b`, `img`, and `h2`. That’s what I like about JSX: it’s familiar. Even
if you don’t know React, you can guess what’s going on here.

But look at the `img` tag: the `src` attribute is dynamic, defined by
`PreloaderImg`, and the `style` attribute takes an object, not a string.
That’s because JSX is more than HTML; it’s JavaScript. Think of props as
function arguments – any valid JavaScript fits.

That will be a cornerstone of our project.

## Step 3: Update App

We use our new Preloader component in App – `src/App.js`. Let’s remove
the `create-react-app` defaults and import our `Preloader` component.

    // src/App.js
    
    import React from 'react';
    // markua-start-delete
    import logo from './logo.svg';
    import './App.css';
    // markua-end-delete
    
    // markua-start-insert
    import Preloader from './components/Preloader';
    // markua-end-insert
    
    class App extends React.Component {
        // markua-start-delete
      render() {
        return (
          <div className="App">
            <div className="App-header">
              <img src={logo} className="App-logo" alt="logo" />
              <h2>Welcome to React</h2>
            </div>
            <p className="App-intro">
              To get started, edit <code>src/App.js</code> and save to reload.
            </p>
          </div>
        );
      }
        // markua-end-delete
    }
    
    export default App;
