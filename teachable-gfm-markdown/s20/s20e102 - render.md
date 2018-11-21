
#### render

Hard work is done. Here’s how you render:

``` javascript
// src/components/Alphabet/Letter.js
    render() {
        const { x, y, fillOpacity, color } = this.state,
            { letter } = this.props;

        return (
            <Transition
                in={this.props.in}
                unmountOnExit={false}
                timeout={750}
                onEnter={this.onEnter}
                onExit={this.onExit}
            >
                <text
                    dy=".35em"
                    x={x}
                    y={y}
                    style={{
                        fillOpacity: fillOpacity,
                        fill: color,
                        font: "bold 48px monospace"
                    }}
                    ref={this.letterRef}
                >
                    {letter}
                </text>
            </Transition>
        );
    }
```

We render a `Transition` element, which gives us the transition super
powers we need to run enter/exit transitions. Update transitions work on
all React components.

The outside `TransitionGroup` gives us the correct `in` prop value, we
just have to pass it into `Transition`. We disable `unmountOnExit` to
make transitions smoother, define a `timeout` which has to match what
we’re using in our transitions, and define `onEnter` and `onExit`
callbacks.

There’s a lot more to the API that we can use and you should check that
out in the docs. Docs don’t go into detail on everything, but if you
experiment I’m sure you’ll figure it out.

Inside the transition we render an SVG `<text>` element rendered at an
`(x, y)` position with a `color` and `fillOpacity` style. It shows a
single letter taken from the `letter` prop.
