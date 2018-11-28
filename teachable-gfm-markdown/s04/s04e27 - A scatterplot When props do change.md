
## Props might update

The story is a little different when our props might update. Since we’re
using D3 objects to calculate SVG properties, we have to make sure those
objects are updated *before* we render.

No problem in React 15: Update in `componentWillUpdate`. But since React
16.3 we’ve been told never to use that again. Causes problems for modern
async rendering.

The official recommended solution is that anything that used to go in
`componentWillUpdate`, can go in `componentDidUpdate`. But not so fast\!

Updating D3 objects in `componentDidUpdate` would mean our visualization
always renders one update behind. Stale renders\! 😱

The new `getDerivedStateFromProps` to the rescue. Our integration
follows a 3-step pattern:

  - set up D3 objects in component state
  - update D3 objects in `getDerivedStateFromProps`
  - output SVG in `render()`

`getDerivedStateFromProps` is officially discouraged, and yet the best
tool we have to make sure D3 state is updated *before* we render.

Because React calls `getDerivedStateFromProps` on every component
render, not just when our props actually change, you should avoid
recalculating complex things too often. Use memoization helpers, check
for changes before updating, stuff like that.

### An updateable scatterplot

Let’s update our scatterplot so it can deal with resizing and updating
data.

3 steps 👇

  - add an interaction that resizes the scatterplot
  - move scales to state
  - update scales in `getDerivedStateFromProps`

You can see [my final solution on
CodeSandbox](https://codesandbox.io/s/ll9kp8or0l). I recommend you
follow along updating your existing
code.

<iframe src="https://codesandbox.io/embed/ll9kp8or0l?codemirror=1&amp;view=split" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin">

</iframe>

#### Resize scatterplot on click

To test our scatterplot’s adaptability, we have to add an interaction:
Resize the scatterplot on click.

That change happens in `App.js`. Click on the `<svg>`, reduce width and
height by 30%.

Move sizing into App state and add an `onClick` handler.

``` javascript
// App.js
class App extends React.Component {
  state = {
    width: 300,
    height: 300
  };

  onClick = () => {
    const { width, height } = this.state;
    this.setState({
      width: width * 0.7,
      height: height * 0.7
    });
  };

  render() {
    const { width, height } = this.state;
```

We changed our App component from a function to a class, added `state`
with default `width` and `height`, and an `onClick` method that reduces
size by 30%. The `render` method reads `width` and `height` from state.

Now gotta change rendering to listen to these values and fire the
`onClick` handler.

``` javascript
// App.js

<svg width="800" height="800" onClick={this.onClick}>
  <Scatterplot
    x={50}
    y={50}
    width={width}
    height={height}
    data={data}
  />
</svg>
```

Similar rendering as before. We have an `<svg>` that contains a
`<Scatterplot>`. The svg fires `this.onClick` on click events and the
scatterplot uses our `width` and `height` values for its props.

If you try this code now, you should see a funny effect where axes move,
but the scatterplot doesn’t resize.

![Axes move, scatterplot doesn’t
resize](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/not-resizing-scatterplot.png)

Peculiar isn’t it? Try to guess why.

#### Move scales to state

The horizontal axis moves because it’s render at `height` vertical
coordinate. Datapoints don’t move because the scales that position them
are calculated once – on component mount.

First step to keeping scales up to date is to move them from component
values into state.

``` javascript
// Scatterplot.js
class Scatterplot extends React.Component {
  state = {
    xScale: d3
      .scaleLinear()
      .domain([0, 1])
      .range([0, this.props.width]),
    yScale: d3
      .scaleLinear()
      .domain([0, 1])
      .range([this.props.height, 0])
  };
```

Same scale definition code we had before. Linear scales, domain from 0
to 1, using props for ranges. But now they’re wrapped in a `state = {}`
object and it’s `xScale: d3 ...` instead of `xScale = d3 ...`.

Our render function should use these as well. Small change:

``` javascript
// Scatterplot.js
    render() {
    const { x, y, data, height } = this.props,
      { yScale, xScale } = this.state;

    return (
      <g transform={`translate(${x}, ${y})`}>
        {data.map(([x, y]) => <circle cx={xScale(x)} cy={yScale(y)} r="5" />)}
```

We use destructuring to take our scales from state, then use them when
mapping over our data.

Clicking on the SVG produces the same result as before, but we’re almost
there. Just one more step.

#### Update scales in `getDerivedStateFromProps`

Last step is to update our scales’ ranges in `getDerivedStateFromProps`.
This method runs every time React touches our component for any reason.

``` javascript
// Scatterplot.js
class Scatterplot extends React.PureComponent {
    // ..
  static getDerivedStateFromProps(props, state) {
    const { yScale, xScale } = state;

    yScale.range([props.height, 0]);
    xScale.range([0, props.width]);

    return {
      ...state,
      yScale,
      xScale
    };
  }
```

Take scales from state, update ranges with new values, return new state.
Nice and easy.

Notice that `getDerivedStateFromProps` is a static method shared by all
instances of our Scatterplot component. You have no reference to a
`this` and have to calculate new state purely from the `props` and
`state` passed into your method.

It’s a lot like a Redux reducer, if that helps you think about it. If
you don’t know what Redux reducers are, don’t worry. Just remember to
return a new version of component state.

Your Scatterplot should now update its size on every click.

![Scatterplot
resizes](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/scatterplot-resizes.png)
