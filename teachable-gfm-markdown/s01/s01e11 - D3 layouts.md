
### 3\) D3 layouts

Sure `.enter.append` looks like magic, but D3 layouts are the real
mind=blown of the D3 ecosystem. They take your input data and return a
full-featured visualization thing.

For example, a force layout using forces between nodes to place them on
the screen.

![Force
layout](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/force-layout.png)

Or a circle packing layout that neatly packs circles.

![Circle packing
layout](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/circle-packing-layout.png)

I don’t know the maths that goes into most of these. And that’s the
point, you shouldn’t have to\!

Here’s a key insight about the magic of layouts: They’re the data part.

You take a `forceLayout` and feed it your data. It returns an object
with a `tick` event callback.

``` javascript
var simulation = d3.forceSimulation()
    .force("link", d3.forceLink().id(function(d) { return d.id; }))
    .force("charge", d3.forceManyBody())
    .force("center", d3.forceCenter(width / 2, height / 2));
```

This `simulation` now handles everything *about* rendering your nodes.
Changes their positions on every `tick` callback, figures out how often
to change stuff, etc.

But it is up to you to render them. A layout handles your dataviz in the
abstract. You’re in control of the rendering.

For a force layout, you have to update the DOM on every tick of the
animation. For circle packing, you render it once.

Once you grok this, all the fancy visualizations out there start making
sense. Also means you can use these fancy layouts in React 🙌
