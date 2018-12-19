
## Overlay a median household line

Here’s a more interesting component: the median dotted line. It shows a
direct comparison between the histogram’s distribution and the median
household income in an area. I’m not sure people understand it at a
glance, but I think it’s cool.

We’re using a quick approach where everything fits into a functional
React component. It’s great for small components like this.

### Step 1: App.js

Inside `src/App.js`, we first have to add an import, then extract the
median household value from state, and in the end, add `MedianLine` to
the render method.

Let’s see if we can do it in a single code block 😄

``` javascript
// src/App.js
import Histogram from './components/Histogram';
import { Title, Description, GraphDescription } from './components/Meta';
// Insert the line(s) between here...
import MedianLine from './components/MedianLine';
// ...and here.

class App extends Component {
    // ...
    render() {
        // ...
        let zoom = null,
            // Insert the line(s) between here...
            medianHousehold = this.state.medianIncomesByUSState['US'][0]
                                  .medianIncome;
            // ...and here.

        return (
            // ...
            <svg width="1100" height="500">
                <CountyMap // ... />
                <Histogram // ... />
                // Insert the line(s) between here...
                <MedianLine data={filteredSalaries}
                            x={500}
                            y={10}
                            width={600}
                            height={500}
                            bottomMargin={5}
                            median={medianHousehold}
                            value={d => d.base_salary} />
                // ...and here.
            </svg>
        )
    }
}
```

You probably don’t remember `medianIncomesByUSState` anymore. We set it
up way back when [tying datasets
together](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6888897#tie-datasets-together).
It groups our salary data by US state.

See, using good names helps 😄

When rendering `MedianLine`, we give it sizing and positioning props,
the dataset, a `value` accessor, and the median value to show. We could
make it smart enough to calculate the median, but the added flexibility
of a prop felt right.

### Step 2: MedianLine

The `MedianLine` component looks similar to what you’ve seen so far.
Some imports, a `constructor` that sets up D3 objects, an `updateD3`
method that keeps them in sync, and a `render` method that outputs SVG.

``` javascript
// src/components/MedianLine.js

import React from "react";
import * as d3 from "d3";

const MedianLine = ({
    data,
    value,
    width,
    height,
    x,
    y,
    bottomMargin,
    median
}) => {
    
};

export default MedianLine;
```

We have some imports, a functional `MedianLine` component that takes our
props, and an export. It should cause an error because it’s not
returning anything.

Everything we need to render the line, fits into this function.

``` javascript
// src/components/MedianLine.js

const MedianLine = ({
    // ...
}) => {
    const yScale = d3
            .scaleLinear()
            .domain([0, d3.max(data, value)])
            .range([height - y - bottomMargin, 0]),
        line = d3.line()([[0, 5], [width, 5]]);

    const medianValue = median || d3.median(data, value);

    const translate = `translate(${x}, ${yScale(medianValue)})`,
        medianLabel = `Median Household: $${yScale.tickFormat()(median)}`;

    return (
        <g className="mean" transform={translate}>
            <text
                x={width - 5}
                y="0"
                textAnchor="end"
                style={{ background: "purple" }}
            >
                {medianLabel}
            </text>
            <path d={line} />
        </g>
    );
};
```

We start with a scale for vertical positioning – `yScale`. It’s linear,
takes values from `0` to `max`, and translates them to pixels less some
margin. For the `medianValue`, we use props, or calculate our own, if
needed. Just like I promised.

A `translate` SVG transform helps us position our line and label. We use
it all to return a `<g>` grouping element containing a `<text>` for our
label, and a `<path>` for the line.

Building the `d` attribute for the path, that’s interesting. We use a
`line` generator from D3.

``` javascript
line = d3.line()([[0, 5], [width, 5]]);
```

It comes from the [d3-shape](https://github.com/d3/d3-shape#lines)
package and generates splines, or polylines. By default, it takes an
array of points and builds a line through all of them. A line from
`[0, 5]` to `[width, 5]` in our case.

That makes it span the entire width and leaves 5px for the label. We’re
using a `transform` on the entire group to vertically position the final
element.

Remember, we already styled `medianLine` when we built [histogram
styles](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6888921#histogram-css)
earlier.

``` css
.mean text {
    font: 11px sans-serif;
    fill: grey;
}

.mean path {
    stroke-dasharray: 3;
    stroke: grey;
    stroke-width: 1px;
}
```

The `stroke-dasharray` is what makes it dashed. `3` means each `3px`
dash is followed by a `3px` blank. You can use [any pattern you
like](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/stroke-dasharray).

You should see a median household salary line overlaid on your
histogram.

![Median line over
histogram](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/dataviz-with-everything.png)

Almost everyone in tech makes more than an entire median household.
Crazy, huh? I think it is.

If that didn’t work, consult the [diff on
Github](https://github.com/Swizec/react-d3js-step-by-step/commit/1fd055e461184fb8dc8dd509edb3a6a683c995fe).
