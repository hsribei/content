
#### componentDidUpdate

`componentDidUpdate` is our trickiest transition yet. It has two jobs:

  - jump existing components to correct horizontal position when a new
    `enter` transition begins
  - transition components into new horizontal positions based on
    changing indexes

It goes like this 👇

``` javascript
// src/components/Alphabet/Letter.js
    componentDidUpdate(prevProps, prevState) {
        if (prevProps.in !== this.props.in && this.props.in) {
            // A new enter transition has begun
            this.setState({
                x: this.props.index * 32
            });
        } else if (prevProps.index !== this.props.index) {
            // Letter is moving to a new location
            let node = d3.select(this.letterRef.current),
                targetX = this.props.index * 32;

            node.style("fill", UpdateColor)
                .transition()
                .duration(750)
                .ease(d3.easeCubicInOut)
                .attr("x", targetX)
                .on("end", () =>
                    this.setState({
                        x: targetX,
                        color: UpdateColor
                    })
                );
        }
    }
```

When the `in` prop changes to `true`, we’re starting a new enter
transition on an existing component. We already moved it to the top of
the visualization after exiting, but we couldn’t have known its future
index.

A quick `setState` makes sure our letter is in the right place and
`onEnter` takes care of the rest.

Otherwise we check if index changed and if it has, we run a transition
in much the same way as we have so far:

  - calculate new `targetX`
  - update letter color
  - start a transition with the usual parameters
  - update `x` coordinate
  - update state when transition ends

You now have a component that knows how to run its own enter/update/exit
transitions. Time to wire it all up in the `render` method.
