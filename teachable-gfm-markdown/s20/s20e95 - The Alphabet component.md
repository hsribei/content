
### The Alphabet component

The `Alphabet` component holds a list of letters in component state and
renders a collection of `Letter` components in a loop.

We start with a skeleton like this:

``` javascript
// src/components/Alphabet.js
import React from "react";
import * as d3 from "d3";
import { TransitionGroup } from "react-transition-group";

import Letter from "./Letter";

class Alphabet extends React.Component {
    static letters = "abcdefghijklmnopqrstuvwxyz".split("");
    state = { alphabet: [] };

    componentDidMount() {
        // start interval
    }

    shuffleAlphabet = () => {
       // generate new alphabet
    };

    render() {
        let transform = `translate(${this.props.x}, ${this.props.y})`;

        return (
            // spit out letters
        );
    }
}

export default Alphabet;
```

We import dependencies and define the `Alphabet` component. It keeps a
list of available letters in a static `letters` property and an empty
`alphabet` in component state.

We’ll start a `d3.interval` on `componentDidMount` and use
`shuffleAlphabet` to generate alphabet subsets.

To showcase enter-update-exit transitions, we create a new alphabet
every second and a half. Using `d3.interval` lets us do that in a
browser friendly way.

``` javascript
// src/components/Alphabet/index.js
    componentDidMount() {
        d3.interval(this.shuffleAlphabet, 1500);
    }

    shuffleAlphabet = () => {
        const alphabet = d3
            .shuffle(Alphabet.letters)
            .slice(0, Math.floor(Math.random() * Alphabet.letters.length))
            .sort();

        this.setState({
            alphabet
        });
    };
```

Think of this as our game loop: Change alphabet state in consistent time
intervals.

We use `d3.interval( //.., 1500)` to call `shuffleAlphabet` every 1.5
seconds. Same as `setInterval`, but friendlier to batteries and CPUs
because it pegs to `requestAnimationFrame`. On each period, we use
`shuffleAlphabet` to shuffle available letters, slice out a random
amount, sort them, and update component state with `setState`.

This process ensures our alphabet is both random and in alphabetical
order.

Starting the interval in `componentDidMount` ensures it only runs when
our Alphabet is on the page. In real life you should stop it on
`componentWillUnmount`. Since this is a tiny experiment and we know
`<Alphabet>` never unmounts without a page refresh, it’s okay to skip
that
step.
