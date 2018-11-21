
### The Letter component

We’re ready for the component that can transition itself into and out of
a visualization. Without consumers having to worry about what’s going on
behind the scenes 👌

The skeleton for our `Letter` component looks like this:

``` javascript
// src/components/Letter.js

import React from "react";
import * as d3 from "d3";
import Transition from "react-transition-group/Transition";

const ExitColor = "brown",
    UpdateColor = "#333",
    EnterColor = "green";

class Letter extends React.Component {
    defaultState = {
        y: -60,
        x: this.props.index * 32,
        color: EnterColor,
        fillOpacity: 1e-6
    };
    state = this.defaultState;
    letterRef = React.createRef();

    onEnter = () => {
        // Letter enters the visualization
    };

    onExit = () => {
        // Letter drops out transition
    };

    componentDidUpdate(prevProps, prevState) {
        // update transition
    }

    render() {
        const { x, y, fillOpacity, color } = this.state,
            { letter } = this.props;

        return (
            // render Transition with text
        );
    }
}

export default Letter;
```

We start with some imports and define a `Letter` component with a
default state. We keep `defaultState` in a separate value because we’re
going to manually reset state in some cases.

A `letterRef` helps us hand over control to D3 during transitions, the
`onEnter` callback handles enter transitions, `onExit` exit transitions,
and `componentDidUpdate` update transitions. Render is where it call
comes together.

Each of these transition methods is going to follow the same approach
you learned about in the swipe transition example. Render from state,
transition with D3, update state to match.

You can make this component more flexible by moving the various magic
numbers we use into props. Default `y` offset, transition duration,
colors, stuff like that. The world is your oyster my friend.
