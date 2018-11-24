
## Step 1: Prep App.js

Let’s set up our `App` component first. That way you’ll see results as
soon data loading starts to work.

Start by importing our data loading method - `loadAllData` - and both D3
and Lodash. We’ll need them later.

``` javascript
// src/App.js
import React from 'react';
// markua-start-insert
import * as d3 from 'd3';
import _ from 'lodash';
// markua-end-insert

import Preloader from './components/Preloader';
// markua-start-insert
import { loadAllData } from './DataHandling';
// markua-end-insert
```

You already know about default imports. Importing with `{}` is how we
import named exports. That lets us get multiple things from the same
file. You’ll see the export side in Step 2.

Don’t worry about the missing `DataHandling` file. It’s coming soon.

``` javascript
// src/App.js
class App extends React.Component {
    state = {
        techSalaries: [],
        // markua-start-insert
        medianIncomes: [],
                countyNames: [],
    };

    componentDidMount() {
        loadAllData(data => this.setState(data));
    }
    // markua-end-insert
```

We initiate data loading inside the `componentDidMount` lifecycle hook.
It fires when React first mounts our component into the DOM.

I like to tie data loading to component mounts because it means you
aren’t making requests you’ll never use. In a bigger app, you’d use
Redux, MobX, or similar to decouple loading from rendering. Many reasons
why.

To load our data, we call the `loadAllData` function, then use
`this.setState` in the callback. This updates `App`’s state and triggers
a re-render, which updates our entire visualization via props.

Yes, it *would* be better to make `loadAllData` return a promise instead
of expecting a callback. That was giving me trouble for some reason and
it doesn’t matter right now anyway.

We also add two more entries to our `state`: `countyNames`, and
`medianIncomes`. Defining what’s in your component state in advance
makes your code easier to read. People, including you, know what to
expect.

Let’s change the `render` method to show a message when our data
finishes loading.

``` javascript
// src/App.js
    render() {
     const { techSalaries } = this.state;
        
     if (techSalaries.length < 1) {
         return (
             <Preloader />
         );
      }

    return (
        // markua-start-delete
        <div className="App">
        // markua-end-delete
        // markua-start-insert
        <div className="App container">
            <h1>Loaded {techSalaries.length} salaries</h1>
        // markua-end-insert
        </div>
    );
}
```

We added a `container` class to the main `<div>` and an `<h1>` tag that
shows how many datapoints there are. You can use any valid JavaScript in
curly braces `{}` and JSX will evaluate it. By convention we only use
that ability to calculate display values.

You should now get an error overlay.

![DataHandling.js not found error
overlay](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/datahandling-error.png)

These nice error overlays come with `create-react-app` and make your
code easier to debug. No hunting around in the terminal to see
compilation errors.

Let’s build that file and fill it with our data loading logic.
