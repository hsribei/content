
### Ball.js state transition

What does move our ball, is our transition. We do that in
`componentDidUpdate`.

``` javascript
  componentDidUpdate() {
    let el = d3.select(this.circleRef.current);

    el.transition()
      .duration(800)
      .ease(d3.easeBounceOut)
      .attr("cx", this.props.x)
      .on("end", () =>
        this.setState({
          x: this.props.x
        })
      );
  }
```

React calls `componentDidUpdate` when props or state change. We use this
as an opportunity to run a D3 transition on our DOM node.

Here’s what happens:

1.  Take the node with `d3.select`
2.  Start a transition with `.transition`
3.  Specify 800ms duration with `.duration`
4.  Add an easing function for that beautiful bounce effect with `.ease`
5.  Define which attributes are changing with `.attr`
6.  Use `.on("end")` to sync React worldview with reality after
    transition ends

What’s great about D3 transitions is that D3 figures everything for you.
You can throw as many attributes at a transition as you want. The
attributes can be as complex as you can imagine.

D3’s interpolation methods will figure it out for you. Numbers, strings,
colors, it all works great.

After a transition is done, I like to sync React’s worldview with what
the transition changed. It’s not strictly required but it can sometimes
lead to React freaking out that you moved an element.

Play around. Try adding some more transitions. Make the ball move
diagonally. Make it go right then down. It’s fun. 😊
