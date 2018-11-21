
#### Declarative render for enter/exit transitions

Our declarative enter/exit transitions start in the `render` method.

``` javascript
// src/components/Alphabet/index.js
    render() {
        let transform = `translate(${this.props.x}, ${this.props.y})`;

        return (
            <g transform={transform}>
                <TransitionGroup enter={true} exit={true} component="g">
                    {this.state.alphabet.map((d, i) => (
                        <Letter letter={d} index={i} key={d} />
                    ))}
                </TransitionGroup>
            </g>
        );
    }
```

An SVG transformation moves our alphabet into the specified `(x, y)`
position. We map through `this.state.alphabet` inside a
`<TransitionGroup>` component and render a `<Letter>` component for
every letter. Each `Letter` gets a `letter` prop for the text, an
`index` prop to know where it stands, and a `key` so React can tell them
apart.

#### The key property

The key property is how React identifies components. Pick wrong, and
you’re gonna have a bad time. I spent many hours debugging and writing
workarounds before I realized that basing my key on the index was a Bad
Move™. *Obviously*, you want the letter to stay constant in each
component and the index to change.

That’s how x-axis transitions work.

You move the letter into a specific place in the alphabet. You’ll see
what I mean when we look at the `Letter` component.
