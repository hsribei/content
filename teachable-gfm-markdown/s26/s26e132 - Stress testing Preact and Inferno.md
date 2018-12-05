
## Trying the stress test in Preact and Inferno

The Pythagoras tree example got converted into 6 different UI libraries:
React, Preact, Inferno, Vue, Angular 2, and CycleJS.

Mostly by the creators of those libraries themselves because they’re
awesome and much smarter than I am. You can see the result, in gifs, on
[my blog
here](https://swizec.com/blog/animating-svg-nodes-react-preact-inferno-vue/swizec/7311).

We’re focusing on React-like libraries, so here’s a rundown of changes
required for Preact and Inferno with links to full code.

### Preact

Implemented by Jason Miller, creator of Preact. [Full code on
Github](https://github.com/developit/preact-fractals)

![](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/preact-pythagora.gif)

Jason used the `preact-compat` layer to make Preact pretend that it’s
React. This might impact performance.

What I love about the Preact example is that it uses async rendering to
look smoother. You can see the redraw cycle lag behind the mouse
movement producing curious effects.

I like it.

Here’s how he did it: [full diff on
github](https://github.com/Swizec/react-fractals/compare/master...developit:master)

In `package.json`, he added `preact`, `preact-compat`, and preact
-compat clones for React libraries. I guess you need the latter so you
don’t have to change your imports.

He changed the functional `Pythagoras` component into a stateful
component to enable async rendering.

``` javascript
// src/Pythagoras.js
export default class {
    render(props) {
        return Pythagoras(props);
    }
}
```

And enabled debounced asynchronous rendering:

``` javascript
// src/index.js
import { options } from 'preact';
options.syncComponentUpdates = false;

//option 1:  rIC + setTimeout fallback
let timer;
options.debounceRendering = f => {
    clearTimeout(timer);
    timer = setTimeout(f, 100);
    requestIdleCallback(f);
};
```

My favorite part is that you can use Preact as a drop-in replacement for
React and it Just Works™ *and* works well. This is promising for anyone
who wants a quick win in their codebase.

### Inferno

Implemented by Dominic Gannaway, creator of Inferno. [Full code on
Github](https://github.com/trueadm/inferno-fractals)

You *can* use Inferno as a drop-in replacement for React, and I did when
I tried to build this myself. Dominic says that impacts performance,
though, so he made a proper fork. You can see the [full diff on
Github](https://github.com/Swizec/react-fractals/compare/master...trueadm:master)

Dominic changed all `react-scripts` references to `inferno-scripts`, and
it’s a good sign that such a thing exists. He also changed the `react`
dependency to `inferno-beta36`, but I’m sure it’s out of beta by the
time you’re reading this. The experiment was done in December 2016.

From there, the main changes are to the imports – React becomes Inferno
– and he changed some class methods to bound fat arrow functions.
Probably a stylistic choice.

He also had to change a string-based ref into a callback ref. Inferno
doesn’t do string-based refs for performance reasons, and we need refs
so we can use D3 to detect mouse position on SVG.

``` javascript
// src/App.js

class App extends Component {
    // ...
    svgElemeRef = (domNode) => {
        this.svgElement = domNode;
    }
    // ...
    render() {
        // ..
        <svg width={this.svg.width} height={this.svg.height} ref={this.svgElemeRef }>
    }
```

In the core `Pythagoras` component, he added two Inferno-specific props:
`noNormalize` and `hasNonKeyedChildren`.

According to [this
issue](https://github.com/trueadm/inferno/issues/565), `noNormalize` is
a benchmark-focused flag that improves performance, and I can’t figure
out what `hasNonKeyedChildren` does. I assume both are performance
optimizations for the Virtual DOM diffing algorithm.
