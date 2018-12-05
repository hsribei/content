
## Stress test your framework with a recursive fractal

To show you how these speed improvements look in real life, I’ve devised
a stress test. A \[pythagorean fractal
tree\](https://en.wikipedia.org/wiki/Pythagoras\_tree\_(fractal) that
moves around when you move your mouse.

![Pythagorean
tree](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/pythagorean-tree.png)

It’s a great stress test because a render like this is the worst case
scenario for tree-based rendering. You have 2048 SVG nodes, deeply
nested, that all change props and re-render with every change.

You can see [the full code on
Github](https://github.com/Swizec/react-fractals). We’re going to focus
on the recursive `<Pythagoras>` component.

### How you too can build a dancing tree fractal

Equipped with basic trigonometry, you need 3 ingredients to build a
dancing tree:

  - a recursive `<Pythagoras>` component
  - a mousemove listener
  - a memoized next-step-props calculation function

We’re using a `<Pythagoras>` component for each square and its two
children, a D3 mouse listener, and some math that a reader helped me
with. We need D3 because its mouse listeners automatically calculate
mouse position relative to SVG coordinates.
[Memoization](https://en.wikipedia.org/wiki/Memoization) in the math
function helps us keep our code faster.

The `<Pythagoras>` component looks like
this:

``` javascript
const Pythagoras = ({ w, x, y, heightFactor, lean, left, right, lvl, maxlvl }) => {
    if (lvl >= maxlvl || w < 1) {
        return null;
    }

    const { nextRight, nextLeft, A, B } = memoizedCalc({
        w: w,
        heightFactor: heightFactor,
        lean: lean
    });

    let rotate = '';

    if (left) {
        rotate = `rotate(${-A} 0 ${w})`;
    }else if (right) {
        rotate = `rotate(${B} ${w} ${w})`;
    }

    return (
        <g transform={`translate(${x} ${y}) ${rotate}`}>
            <rect width={w} height={w}
                  x={0} y={0}
                  style={{fill: interpolateViridis(lvl/maxlvl)}} />

            <Pythagoras w={nextLeft}
                        x={0} y={-nextLeft}
                        lvl={lvl+1} maxlvl={maxlvl}
                        heightFactor={heightFactor}
                        lean={lean}
                        left />

            <Pythagoras w={nextRight}
                        x={w-nextRight} y={-nextRight}
                        lvl={lvl+1} maxlvl={maxlvl}
                        heightFactor={heightFactor}
                        lean={lean}
                        right />

        </g>
    );
};
```

We break out of recursion when we get too deep or try to draw an
invisible square. Otherwise we:

  - use `memoizedCalc` to do the mathematics
  - define different `rotate()` transforms for the `left` and `right`
    branches
  - return an SVG `<rect>` for the current rectangle, and two
    `<Pythagoras>` elements for each branch.

Most of this code deals with passing props to children, which isn’t the
most elegant approach, but it works. The rest is about positioning
branches so corners match up.

![Corners matching
up](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/pythagoras-corners-match-up.png)

#### The math

I don’t *really* understand this math, but I sort of know where it’s
coming from. It’s the [sine
law](https://en.wikipedia.org/wiki/Law_of_sines) applied correctly. The
part I failed at [when I
tried](https://swizec.com/blog/fractals-react/swizec/7233) to do it
myself.

``` javascript
const memoizedCalc = function () {
    const memo = {};

    const key = ({ w, heightFactor, lean }) => [w,heightFactor, lean].join('-');

    return (args) => {
        const memoKey = key(args);

        if (memo[memoKey]) {
            return memo[memoKey];
        }else{
            const { w, heightFactor, lean } = args;

            const trigH = heightFactor*w;

            const result = {
                nextRight: Math.sqrt(trigH**2 + (w * (.5+lean))**2),
                nextLeft: Math.sqrt(trigH**2 + (w * (.5-lean))**2),
                A: Math.deg(Math.atan(trigH / ((.5-lean) * w))),
                B: Math.deg(Math.atan(trigH / ((.5+lean) * w)))
            };

            memo[memoKey] = result;
            return result;
        }
    }
}();
```

We expanded the basic law of sines with a dynamic `heightFactor` and
`lean` adjustment. We’ll control those with mouse movement.

To improve performance our `memoizedCalc` function has an internal data
store that maintains a hash of every argument tuple and its result. So
when we call `memoizedCalc` with the same arguments multiple times, it
reads from cache instead of recalculating.

At 11 levels of recursion, `memoizedCalc` is called 2,048 times and only
returns 11 different results. Great candidate for memoization if I ever
saw one.

Of course, a benchmark would be great here. Maybe `sqrt`, `atan`, and
`**` aren’t *that* slow, and our real bottleneck is redrawing all those
nodes on every mouse move. Hint: it totally is.

Our ability to memoize calculation also means that we could, in theory,
flatten our rendering. Instead of recursion, we could use iteration and
render in layers. Squares at each level are the same, just at different
`(x, y)` coordinates.

This, however, would be far too tricky to implement.

#### The mouse listener

Inside
[`App.js`](https://github.com/Swizec/react-fractals/blob/master/src/App.js),
we add a mouse event listener. We use D3’s because it gives us
SVG-relative position calculation out of the box. With React’s, we’d
have to do the hard work ourselves.

``` javascript
// App.js
state = {
        currentMax: 0,
        baseW: 80,
        heightFactor: 0,
        lean: 0
    };

componentDidMount() {
    d3select(this.refs.svg)
       .on("mousemove", this.onMouseMove.bind(this));
}

onMouseMove(event) {
    const [x, y] = d3mouse(this.refs.svg),

    scaleFactor = scaleLinear().domain([this.svg.height, 0])
                                                         .range([0, .8]),

    scaleLean = scaleLinear().domain([0, this.svg.width/2, this.svg.width])
                                                     .range([.5, 0, -.5]);

    this.setState({
        heightFactor: scaleFactor(y),
        lean: scaleLean(x)
    });
}

// ...

render() {
    // ...
    <svg ref="svg"> //...
    <Pythagoras w={this.state.baseW}
                          h={this.state.baseW}
                          heightFactor={this.state.heightFactor}
                       lean={this.state.lean}
                       x={this.svg.width/2-40}
                       y={this.svg.height-this.state.baseW}
                       lvl={0}
                       maxlvl={this.state.currentMax}/>
}
```

A few things happen here:

  - we set initial `lean` and `heightFactor` to `0`
  - in `componentDidMount`, we use `d3.select` and `.on` to add a mouse
    listener
  - we define an `onMouseMove` method as the listener
  - render the first `<Pythagoras>` using values from `state`

The `lean` parameter tells us which way the tree is leaning and how
much, the `heightFactor` tells us how high those triangles should be. We
control both with the mouse position.

That happens in `onMouseMove`:

``` javascript
onMouseMove(event) {
    const [x, y] = d3mouse(this.refs.svg),

    scaleFactor = scaleLinear().domain([this.svg.height, 0])
                                                         .range([0, .8]),

    scaleLean = scaleLinear().domain([0, this.svg.width/2, this.svg.width])
                                                     .range([.5, 0, -.5]);

    this.setState({
        heightFactor: scaleFactor(y),
        lean: scaleLean(x)
    });
}
```

`d3mouse` - an imported `mouse` function from `d3-selection` - gives us
cursor position relative to the SVG element. Two linear scales give us
`scaleFactor` and `scaleLean` values, which we put into component state.

When we feed a change to `this.setState`, it triggers a re-render of the
entire tree, our `memoizedCalc` function returns new values, and the
final result is a dancing tree.

![Dancing Pythagoras
tree](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/pythagoras-dancing.gif)

Beautious. 😍
