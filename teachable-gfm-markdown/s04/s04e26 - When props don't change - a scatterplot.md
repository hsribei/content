
## Props don’t change

Ignoring props changes makes our life easier, but the component less
flexible and reusable. Great when you know in advance that there are
features you don’t ned to support.

Like, no filtering your data or changing component size 👉 means your D3
scales don’t have to change.

When our props don’t change, we follow a 2-step integration process:

  - set up D3 objects as class properties
  - output SVG in `render()`

We don’t have to worry about updating D3 objects on prop changes. Work
done 👌

### An unchanging scatterplot

We’re building a scatterplot of random data. You can see the [final
solution on
CodeSandbox](https://codesandbox.io/s/1zlp4jv494)

<iframe src="https://codesandbox.io/embed/1zlp4jv494?codemirror=1&amp;view=split" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin">

</iframe>

Here’s the approach 👇

  - stub out the basic setup
  - generate random data
  - stub out Scatterplot
  - set up D3 scales
  - render circles for each entry
  - add axes

I recommend creating a new CodeSandbox, or starting a new app with
create-react-app. They should work the same.

#### Basic setup

Make sure you have `d3` added as a dependency. Then add imports in your
`App.js` file.

``` javascript
// ./App.js

import * as d3 from "d3";
import Scatterplot from "./Scatterplot";
```

Add an `<svg>` and render a Scatterplot in the render method. This will
throw an error because we haven’t defined the Scatterplot yet and that’s
okay.

``` javascript
// ./App.js

function App() {
  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
      <svg width="800" height="800">
        <Scatterplot x={50} y={50} width={300} height={300} data={data} />
      </svg>
    </div>
  );
}
```

CodeSandbox adds most of that code by default. If you’re using
create-react-app, your App component has different markup. That’s okay
too.

We added this part:

``` javascript
<svg width="800" height="800">
    <Scatterplot x={50} y={50} width={300} height={300} data={data} />
</svg>
```

An `<svg>` drawing area with a width and a height. Inside, a
`<Scatterplot` that’s positioned at `(50, 50)` and is 300px tall and
wide. We’ll have to listen to those props when building the Scatterplot.

It also accepts data.

#### Random data

We’re using a line of code to generate data for our scatterplot. Put it
in App.js. Either globally or within the App function. Doesn’t matter
because this is an example.

``` javascript
const data = d3.range(100)
                             .map(_ => [Math.random(), Math.random()]);
```

`d3.range` returns a counting array from 0 to 100. Think `[1,2,3,4
...]`.

We iterate over this array and return a pair of random numbers for each
entry. These will be our X and Y coordinates.

#### Scatterplot

Our scatterplot goes in a new `Scatterplot.js` file. Starts with imports
and an empty React component.

``` javascript
// ./Scatterplot.js
import React from "react";
import * as d3 from "d3";

class Scatterplot extends React.Component {

  render() {
    const { x, y, data, height } = this.props;

    return (
      <g transform={`translate(${x}, ${y})`}>
 
      </g>
    );
  }
}

export default Scatterplot;
```

Import dependencies, create a `Scatterplot` component, render a grouping
element moved to the correct `x` and `y` position. Nothing too strange
yet.

#### D3 scales

Now we define D3 scales as component properties. We’re using the class
field syntax that’s common in React projects.

Technically a Babel plugin, but comes by default with CodeSandbox React
projects and create-react-app setup. As far as I can tell, it’s a common
way to write React components.

``` javascript
// ./Scatterplot.js
class Scatterplot extends React.Component {
  xScale = d3
    .scaleLinear()
    .domain([0, 1])
    .range([0, this.props.width]);
  yScale = d3
    .scaleLinear()
    .domain([0, 1])
    .range([this.props.height, 0]);
```

We’re defining `this.xScale` and `this.yScale` as linear scales. Their
domains go from 0 to 1 because that’s what Math.random returns and their
ranges describe the size of our scatterplot component.

Idea being that these two scales will help us take those tiny variations
in datapoint coordinates and explode them up to the full size of our
scatterplot. Without this, they’d overlap and we wouldn’t see anything.

#### Circles for each entry

Rendering our data points is a matter of looping over the data and
rendering a `<circle>` for each entry. Using our scales to define
positioning.

``` javascript
// ./Scatterplot.js

return (
  <g transform={`translate(${x}, ${y})`}>
    {data.map(([x, y]) => (
      <circle cx={this.xScale(x)} cy={this.yScale(y)} r="5" />
    ))}
  </g>
);
```

In the `return` statement of our `render` render method, we add a
`data.map` with an iterator method. This method takes our datapoint,
uses array destructuring to get `x` and `y` coordinates, then uses our
scales to define `cx` and `cy` attributes on a `<circle>` element.

#### Add axes

You can reuse axes from our earlier exercise. Or copy mine from [the
CodeSandbox](https://codesandbox.io/s/1zlp4jv494)

<iframe src="https://codesandbox.io/embed/1zlp4jv494?codemirror=1&amp;view=split" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin">

</iframe>

Mine take a scale and orientation as props, which makes them more
flexible. Means we can use the same component for both the vertical and
horizontal axis on our Scatterplot.

Put the axis code in `Axis.js`, then augment the Scatterplot like this 👇

``` javascript
import Axis from "./Axis";

// ...

return (
  <g transform={`translate(${x}, ${y})`}>
    {data.map(([x, y]) => (
      <circle cx={this.xScale(x)} cy={this.yScale(y)} r="5" />
    ))}
    <Axis x={0} y={0} scale={this.yScale} type="Left" />
    <Axis x={0} y={height} scale={this.xScale} type="Bottom" />
  </g>
);
```

Vertical axis takes the vertical `this.yScale` scale, orients to the
`Left` and we position it top left. The horizontal axis takes the
horizontal `this.xScale` scale, orients to the `Bottom`, and we render
it bottom left.

Your Scatterplot should now look like this

![Rendered basic
scatterplot](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/scatterplot-basic.png)
