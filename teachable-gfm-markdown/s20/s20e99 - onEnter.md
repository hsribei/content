
#### onEnter

We start with the enter transition in the `onEnter` callback.

``` javascript
// src/components/Letter.js
    onEnter = () => {
        // Letter is entering the visualization
        let node = d3.select(this.letterRef.current);

        node.transition()
            .duration(750)
            .ease(d3.easeCubicInOut)
            .attr("y", 0)
            .style("fill-opacity", 1)
            .on("end", () => {
                this.setState({
                    y: 0,
                    fillOpacity: 1,
                    color: UpdateColor
                });
            });
    };
```

We use `d3.select` to grab our DOM node and take control with D3. Start
a new transition with `.transition()`, specify a duration, an easing
function, and specify the changes. Vertical position moves to `0`,
opacity changes to `1`.

This creates a drop-in fade-in effect.

When our transition ends, we update state with the new `y` coordinate,
`fillOpacity`, and `color`.

The result is an invisible letter that starts at -60px and moves into
0px and full visibility over 750 milliseconds.
