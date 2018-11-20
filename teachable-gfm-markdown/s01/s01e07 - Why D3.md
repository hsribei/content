
D3 is the best library out there for custom data visualization. It comes
with a rich ecosystem of functions for almost anything you can think of.
From simple medians, to automatic axis generators, and force diagrams.

Most data visualization you see on the web is built with D3. The New
York Times uses it, Guardian uses it, r/dataisbeautiful is full of it.

Learning D3 from scratch is where life gets tricky.

There are several gotchas that trip you up and make examples look like
magic. You’ve probably noticed this, if you ever looked at an example
project built with D3. They’re full of spaghetti code, global variables,
and often aren’t made to be maintainable.

Most examples are just one-off toys after all. It’s art.

A lot of dataviz that *isn’t* art, is charts and graphs. You’ll often
find that using D3 to build those, is too complicated. D3 gives you more
power than you need.

If you want charts, I suggest using a charting library.

Where many charting libraries fall short is customization. The API is
limited, you can’t do everything you want, and it gets easier to just
build it yourself.

A lot of what you end up doing in real life is finding a D3 example that
looks like what you want to build and adapting it. That’s why you should
learn at least some D3.

But D3 is hard to read. Take this barchart code, for example 👇

``` javascript
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

Can you tell what’s going on? I’d need to read it pretty carefully.

Which brings us to 👇
