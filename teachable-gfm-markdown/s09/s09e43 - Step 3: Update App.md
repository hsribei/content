
## Step 3: Update App

We use our new Preloader component in App – `src/App.js`. Let’s remove
the `create-react-app` defaults and import our `Preloader` component.

``` javascript
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
```

We removed the logo and style imports, added an import for `Preloader`,
and gutted the `App` class. It’s great for a default app, but we don’t
need that anymore.

Let’s define a default `state` and a `render` method that uses our
`Preloader` component when there’s no data.

``` javascript
// src/App.js

class App extends React.Component {
    // markua-start-insert
    state = {
        techSalaries: []
    }

    render() {
        const { techSalaries } = this.state;
        
        if (techSalaries.length < 1) {
            return (
                <Preloader />
            );
        }

        return (
            <div className="App">

            </div>
        );
    }
    // markua-end-insert
}
```

Nowadays we can define properties directly in the class body without a
constructor method. It’s not part of the official JavaScript standard
yet, but most React codebases use this pattern.

Properties defined this way are bound to each instance of our components
so they have the correct `this` value. Late you’ll see we can use this
shorthand to neatly define event handlers.

We set `techSalaries` to an empty array by default. In `render` we use
object destructuring to take `techSalaries` out of component state,
`this.state`, and check whether it’s empty. When `techSalaries` is empty
our component renders the preloader, otherwise an empty div.

If your `npm start` is running, the preloader should show up on your
screen.

![Preloader without Bootstrap
styles](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/preloader-without-styles-screenshot.png)

Hmm… that’s not very pretty. Let’s fix it.
