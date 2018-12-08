
## Step 3: Histogram component

We’re following the [full-feature
integration](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6887740#full-feature-integration)
approach for our Histogram component. React talks to the DOM, D3
calculates the props.

We’ll use two components:

1.  `Histogram` makes the general layout, dealing with D3, and
    translating raw data into a histogram
2.  `HistogramBar` draws a single bar and labels it

Let’s start with the basics: a `Histogram` directory and an `index.js`
file. Keeps our code organized and imports easy. I like to use
directories for components made of multiple files.

``` javascript
export { default } from "./Histogram";
```

Import and re-export the histogram componenet from `./Histogram`. This
way you can keep your histogram code in a file called Histogram and
pretend the directory itself is exporting it.

Great way to group files that belong together without exposing your
directory’s internal structure.

Now we need the `Histogram.js` file. Start with some imports, a default
export, and a stubbed out `Histogram` class.

``` javascript
// src/components/Histogram/Histogram.js
import React from "react";
import * as d3 from "d3";

class Histogram extends React.Component {
    state = {
        histogram: d3.histogram(),
        widthScale: d3.scaleLinear(),
        yScale: d3.scaleLinear()
    };

    static getDerivedStateFromProps(props, state) {
        let { histogram, widthScale, yScale } = state;

        return {
            ...state,
            histogram,
            widthScale,
            yScale
        };
    }

    makeBar = bar => {
        const { yScale, widthScale } = this.state;

    };

    render() {
        const { histogram, yScale } = this.state,
            { x, y, data, axisMargin } = this.props;
            
        return null;
    }
}
```

We import React and D3, and set up `Histogram`.

Default `state` for our D3 objects: histogram, widthScale, and yScale.
An empty `getDerivedStateFromProps` to keep them updated, `makeBar` to
help us render each bar, and `render` returning null for now.

### getDerivedStateFromProps

``` javascript
// src/components/Histogram/Histogram.js
    static getDerivedStateFromProps(props, state) {
        let { histogram, widthScale, yScale } = state;

        histogram.thresholds(props.bins).value(props.value);

        const bars = histogram(props.data),
            counts = bars.map(d => d.length);

        widthScale
            .domain([d3.min(counts), d3.max(counts)])
            .range([0, props.width - props.axisMargin]);

        yScale
            .domain([0, d3.max(bars, d => d.x1)])
            .range([props.height - props.y - props.bottomMargin, 0]);

        return {
            ...state,
            histogram,
            widthScale,
            yScale
        };
    }
```

First, we configure the `histogram` generator. `thresholds` specify how
many bins we want and `value` specifies the value accessor function. We
get both from props passed into the `Histogram` component.

In our case that makes 20 bins, and the value accessor returns each data
point’s `base_salary`.

We feed the data prop into our histogram generator, and count how many
values are in each bin with a `.map` call. We need those to configure
our scales.

If you print the result of `histogram()`, you’ll see an array structure
where each entry holds metadata about the bin and the values it
contains.

![console.log(this.histogram())](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/histogram-data-screenshot.png)

Let’s use this info to set up our scales.

`widthScale` has a range from the smallest (`d3.min`) bin to the largest
(`d3.max`), and a range of `0` to width less a margin. We’ll use it to
calculate bar sizes.

`yScale` has a range from `0` to the largest `x1` coordinate we can find
in a bin. Bins go from `x0` to `x1`, which reflects the fact that most
histograms are horizontally oriented. Ours is vertical so that our
labels are easier to read. The range goes from `0` to the maximum height
less a margin.

Now let’s render this puppy.

### render

``` javascript
// src/components/Histogram/Histogram.js
class Histogram extends React.Component {
    // ...
    render() {
        const { histogram, yScale } = this.state,
            { x, y, data, axisMargin } = this.props;

        const bars = histogram(data);

        return (
            <g className="histogram" transform={`translate(${x}, ${y})`}>
                <g className="bars">
                    {bars.map(this.makeBar))}
                </g>
            </g>
        );
    }
}
```

We take everything we need out of `state` and `props` with
destructuring, call `histogram()` on our data to get a list of bars, and
render.

Our render method returns a `<g>` grouping element transformed to the
position given in props and walks through the `bars` array, calling
`makeBar` for each. Later, we’re going to add an `Axis` as well.

This is a great example of React’s declarativeness. We have a bunch of
stuff, and all it takes to render is a loop. No worrying about how it
renders, where it goes, or anything like that. Walk through data,
render, done.

### makeBar

`makeBar` is a function that takes a histogram bar’s metadata and
returns a `HistogramBar` component. We use it to make our declarative
loop more readable.

``` javascript
// src/components/Histogram/Histogram.js
class Histogram extends React.Component {
    // ...
    makeBar = bar => {
        const { yScale, widthScale } = this.state;

        let percent = (bar.length / this.props.data.length) * 100;

        let props = {
            percent: percent,
            x: this.props.axisMargin,
            y: yScale(bar.x1),
            width: widthScale(bar.length),
            height: yScale(bar.x0) - yScale(bar.x1),
            key: "histogram-bar-" + bar.x0
        };

        return <HistogramBar {...props} />;
    };
}
```

See, we’re calculating `props` and feeding them into `HistogramBar`.
Putting it in a separate function just makes the `.map` construct in
`render` easier to read. There’s a lot of props to calculate.

Some, like `axisMargin` we pass through, others like `width` and
`height` we calculate using our scales.

Setting the `key` prop is important. React uses it to tell the bars
apart and only re-render those that change.
