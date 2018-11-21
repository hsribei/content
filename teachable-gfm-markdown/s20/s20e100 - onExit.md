
#### onExit

Our exit transition goes in the `onExit` callback.

``` javascript
// src/components/Alphabet/
    onExit = () => {
        // Letter is dropping out
        let node = d3.select(this.letterRef.current);

        node.style("fill", ExitColor)
            .transition(this.transition)
            .attr("y", 60)
            .style("fill-opacity", 1e-6)
            .on("end", () => this.setState(this.defaultState));
    };
```

Same as before, we take control of the DOM, run a transition, and update
state when we’re done. We start with forcing our letter into a new
color, then move it 60px down, transition to invisible, and reset state.

But why are we resetting state instead of updating to current reality?

Our components never unmount.

We avoid unmounts to keep transitions smoother. Instead of unmounting,
we have to reset state back to its default values.

That moves the letter back into its enter state and ensures even re-used
letters drop down from the top. Took me a while to tinker that one out.
