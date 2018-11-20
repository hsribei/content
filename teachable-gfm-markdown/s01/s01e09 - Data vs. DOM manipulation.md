
### 1\) Data manipulation vs. DOM manipulation

All D3 examples are split into two parts:

1.  Data manipulation
2.  DOM manipulation

First you prep your values, then you render.

You have to go through many examples to notice what’s going on.
Inference learning is hard. Most beginners miss this pattern and it
makes D3 look more confusing than it is.

Let’s take an example from [D3’s
docs](https://github.com/d3/d3/wiki/Gallery), a bar chart with a hover
effect.

![An example D3
barchart](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/barchart-example.png)

You can [try it
online](https://cdn.rawgit.com/mbostock/3885304/raw/a91f37f5f4b43269df3dbabcda0090310c05285d/index.html).
When you hover on a bar, it changes color. Pretty neat.

Mike Bostock, the creator of D3, built this chart in 43 lines of code.
Here they are 👇

``` javascript
var svg = d3.select("svg"),
    margin = {top: 20, right: 20, bottom: 30, left: 40},
    width = +svg.attr("width") - margin.left - margin.right,
    height = +svg.attr("height") - margin.top - margin.bottom;

var x = d3.scaleBand().rangeRound([0, width]).padding(0.1),
    y = d3.scaleLinear().rangeRound([height, 0]);

var g = svg.append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

d3.tsv("data.tsv", function(d) {
  d.frequency = +d.frequency;
  return d;
}, function(error, data) {
  if (error) throw error;

  x.domain(data.map(function(d) { return d.letter; }));
  y.domain([0, d3.max(data, function(d) { return d.frequency; })]);

  g.append("g")
      .attr("class", "axis axis--x")
      .attr("transform", "translate(0," + height + ")")
      .call(d3.axisBottom(x));

  g.append("g")
      .attr("class", "axis axis--y")
      .call(d3.axisLeft(y).ticks(10, "%"))
    .append("text")
      .attr("transform", "rotate(-90)")
      .attr("y", 6)
      .attr("dy", "0.71em")
      .attr("text-anchor", "end")
      .text("Frequency");

  g.selectAll(".bar")
    .data(data)
    .enter().append("rect")
      .attr("class", "bar")
      .attr("x", function(d) { return x(d.letter); })
      .attr("y", function(d) { return y(d.frequency); })
      .attr("width", x.bandwidth())
      .attr("height", function(d) { return height - y(d.frequency); });
});
```

There are two parts to this code: Data manipulation and DOM
manipulation.

``` javascript
var // ..,
    margin = {top: 20, right: 20, bottom: 30, left: 40},
    width = +svg.attr("width") - margin.left - margin.right,
    height = +svg.attr("height") - margin.top - margin.bottom;

var x = d3.scaleBand().rangeRound([0, width]).padding(0.1),
    y = d3.scaleLinear().rangeRound([height, 0]);

// ...

d3.tsv("data.tsv", function(d) {
  d.frequency = +d.frequency;
  return d;
}, function(error, data) {
  if (error) throw error;

  x.domain(data.map(function(d) { return d.letter; }));
  y.domain([0, d3.max(data, function(d) { return d.frequency; })]);

// ...
});
```

Bostock here first prepares his data:

  - some sizing variables (`margin`, `width`, `height`)
  - two scales to help with data-to-coordinates conversion (`x, y`)
  - loads his dataset (`d3.tsv`) and updates his scales’ domains

In the DOM manipulation part, he puts shapes and objects into an SVG.
This is the part that shows up in your browser.

``` javascript
var svg = d3.select("svg"),
    // ..
    
// ..

var g = svg.append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

// ..
  g.append("g")
      .attr("class", "axis axis--x")
      .attr("transform", "translate(0," + height + ")")
      .call(d3.axisBottom(x));

  g.append("g")
      .attr("class", "axis axis--y")
      .call(d3.axisLeft(y).ticks(10, "%"))
    .append("text")
      .attr("transform", "rotate(-90)")
      .attr("y", 6)
      .attr("dy", "0.71em")
      .attr("text-anchor", "end")
      .text("Frequency");

  g.selectAll(".bar")
    .data(data)
    .enter().append("rect")
      .attr("class", "bar")
      .attr("x", function(d) { return x(d.letter); })
      .attr("y", function(d) { return y(d.frequency); })
      .attr("width", x.bandwidth())
      .attr("height", function(d) { return height - y(d.frequency); });
});
```

DOM manipulation in D3 happens via D3 selections. They’re a lot like
jQuery `$(something)`. This is the part we’re doing with React later on.

Here Bostock does a few things

  - selects the `<svg>` node (`d3.select`)
  - appends a grouping `<g>` node (`.append`) with an SVG positioning
    attribute (translate)
  - adds a bottom axis by appending a `<g>`, moving it, then calling
    `d3.axisBottom` on it. D3 has built-in axis generators
  - adds a left axis using the same approach but rotating the ticks
  - appends a text label “Frequency” to the left axis
  - uses `selectAll.data` to make a virtual selection of `.bar` nodes
    and attach some data, then for every new data value (.enter),
    appends a `<rect>` node and gives it attributes

That last part is where people get lost. It looks like magic. Even to
me.

It’s a declarative approach to rendering data. Works great, hard to
understand. That’s why we’ll do it in React instead :)

You can think of `.enter` as a loop over your data. Everything chained
after `.enter` is your loop’s body. Sort of like doing `data.map(d =>
append(rect).setManyAttributes())`

That function executes for any *new* data “entering” your visualization.
There’s also `.exit` for anything that’s dropping out, and `.update` for
anything that’s changing.
