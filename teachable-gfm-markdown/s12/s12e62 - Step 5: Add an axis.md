
## Step 5: Axis HOC

Our histogram is pretty, but it needs an axis to be useful. You’ve
already learned how to implement an axis when we talked about [blackbox
integration](#blackbox-axis). We’re going to use the same approach and
copy those concepts into the real project.

### D3blackbox

We start with the D3blackbox higher order component. Same as before,
except we put it in `src/components`.

``` javascript
import React from "react";

export default function D3blackbox(D3render) {
    return class Blackbox extends React.Component {
        anchorRef = React.createRef();

        componentDidMount() {
            D3render.call(this);
        }
        componentDidUpdate() {
            D3render.call(this);
        }

        render() {
            const { x, y } = this.props;
            return (
                <g transform={`translate(${x}, ${y})`} ref={this.anchorRef} />
            );
        }
    };
}
```

Take a `D3render` function, call it on `componentDidMount` and
`componentDidUpdate`, and render a positioned anchor element for
`D3render` to hook into.

### Axis component

With `D3blackbox`, we can reduce the `Axis` component to a wrapped
function. We’re implementing the `D3render` method.

``` javascript
import * as d3 from "d3";
import D3blackbox from "../D3blackbox";

const Axis = D3blackbox(function() {
    const axis = d3
        .axisLeft()
        .tickFormat(d => `${d3.format(".2s")(d)}`)
        .scale(this.props.scale)
        .ticks(this.props.data.length);

    d3.select(this.anchorRef.current).call(axis);
});

export default Axis;
```

We use D3’s `axisLeft` generator, configure its `tickFormat`, pass in a
`scale` from our props, and specify how many `ticks` we want. To render,
we `select` the anchor element from `D3blackbox` and `call` the axis
generator on it.

Yes, this `Axis` works just for our specific use case and that’s okay.
No need to generalize your code until you know where else you’re using
it.

Remember the [YAGNI
principle](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it).

### Add Axis to Histogram

To render our new `Axis`, we add it to the `Histogram` component. It’s a
two step process:

1.  Import `Axis` component
2.  Render it

<!-- end list -->

``` javascript
// src/components/Histogram/Histogram.js
import React, { Component } from 'react';
import * as d3 from 'd3';

// markua-start-insert
import Axis from './Axis';
// markua-end-insert

// ...
class Histogram extends Component {
    // ...
    render() {
        const { histogram, yScale } = this.state,
            { x, y, data, axisMargin } = this.props;
            
        const bars = histogram(data);

        return (
            <g className="histogram" transform={translate}>
                <g className="bars">
                    {bars.map(this.makeBar)}
                </g>
                // markua-start-insert
                <Axis x={axisMargin-3}
                      y={0}
                      data={bars}
                      scale={yScale} />
                // markua-end-insert
            </g>
        );
    }
```

We import our `Axis` and add it to the `render` method with some props.
It takes an `x` and `y` coordinate, the `data`, and a `scale`.

An axis appears.

![Basic histogram with
axis](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/basic-histogram.png)

If that didn’t work, try comparing your changes to this [diff on
Github](https://github.com/Swizec/react-d3js-step-by-step/commit/02a40899e348587a909e97e8f18ecf468e2fe218).
