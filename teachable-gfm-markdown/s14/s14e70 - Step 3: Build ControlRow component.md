
## Step 3: Build ControlRow component

Let’s build the `ControlRow` component. It renders a row of controls and
ensures only one at a time is selected.

We’ll start with a stub and go from there.

``` javascript
// src/components/Controls/ControlRow.js
import React from "react";

import Toggle from "./Toggle";

class ControlRow extends React.Component {
    makePick = (picked, newState) => {
 
    };

    _addToggle(name) {
    }

    render() {
        const { toggleNames } = this.props;
    }
}

export default ControlRow;
```

We start with imports, big surprise, then make a stub with 3 methods.
Can you guess what they are?

  - `makePick` is the `Toggle` click callback
  - `_addToggle` is a rendering helper method
  - `render` renders a row of buttons

<!-- end list -->

``` javascript
// src/components/Controls/ControlRow.js

class ControlRow extends React.Component {
    makePick = (picked, newState) => {
        // if newState is false, we want to reset
        this.props.updateDataFilter(picked, !newState);
    };
```

`makePick` calls the data filter update and passes in the new value and
whether we want to unselect. Pretty simple right?

``` javascript
// src/components/Controls/ControlRow.js
class ControlRow extends React.Component {
    // ...

    _addToggle(name) {
        let key = `toggle-${name}`,
            label = name;

        if (this.props.capitalize) {
            label = label.toUpperCase();
        }

        return (
            <Toggle
                label={label}
                name={name}
                key={key}
                value={this.props.picked === name}
                onClick={this.makePick}
            />
        );
    }

    render() {
        const { toggleNames } = this.props;

        return (
            <div className="row">
                <div className="col-md-12">
                    {toggleNames.map(name => this._addToggle(name))}
                </div>
            </div>
        );
    }
}
```

Rendering comes in two functions: `_addToggle`, which is a helper, and
`render`, which is the main render.

In `render`, we set up two `div`s with Bootstrap classes. The first is a
`row`, and the second is a full-width column. I tried using a column for
each button, but it was annoying to manage and used too much space.

Inside the divs, we map over all toggles and use `_addToggle` to render
each of them.

`_addToggle` renders a `Toggle` component with a `label`, `name`,
`value` and `onClick` callback. The label is just a prettier version of
the name, which also serves as a key in our `toggleValues` dictionary.
It’s going to be the `picked` attribute in `makePick`.

Your browser should continue showing an error, but it should change to
talking about the `Toggle` component instead of
`ControlRow`.

![](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/toggle-error.png)

Let’s build it.
