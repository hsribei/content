
## A quick blackbox example - a D3 axis

Let’s build an axis component. Axes are the perfect use-case for
blackbox components. D3 comes with an axis generator bundled inside, and
they’re difficult to build from scratch.

They don’t *look* difficult, but there are many tiny details you have to
get *just right*.

D3’s axis generator takes a scale and some configuration to render an
axis for us. The code looks like this:

    const scale = d3.scaleLinear()
            .domain([0, 10])
            .range([0, 200]);
    const axis = d3.axisBottom(scale);
    
    d3.select('svg')
      .append('g')
      .attr('transform', 'translate(10, 30)')
      .call(axis);

You can [try it out on
CodeSandbox](https://codesandbox.io/s/v6ovkow8q3).

If this code doesn’t make any sense, don’t worry. There’s a bunch of D3
to learn, and I’ll help you out. If it’s obvious, you’re a pro\! This
book will be much quicker to read.

We start with a linear scale that has a domain `[0, 10]` and a range
`[0, 200]`. Scales are like mathematical functions that map a domain to
a range. In this case, calling `scale(0)` returns `0`, `scale(5)`
returns `100`, `scale(10)` returns `200`. Just like a linear function
from math class – y = kx + n.

We create an axis generator with `axisBottom`, which takes a `scale` and
creates a `bottom` oriented axis – numbers below the line. You can also
change settings for the number of ticks, their sizing, spacing, and so
on.

Equipped with an `axis` generator, we `select` the `svg` element, append
a grouping element, use a `transform` attribute to move it `10`px to the
right and `30`px down, and invoke the generator with `.call()`.

It creates a small axis:

![Simple
axis](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/simple-axis.png)

Play around with it on
[Codesandbox](https://codepen.io/swizec/pen/YGoYBM). Change the scale
type, play with axis orientation. Use `.ticks` on the axis to change how
many show up. Have some fun :)
