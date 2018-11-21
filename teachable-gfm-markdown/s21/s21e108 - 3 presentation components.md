
## 3 presentation components

We start with the presentation components because they’re the simplest.
To render a collection of particles, we need:

  - a stateless `Particle`
  - a stateless `Particles`
  - a class-based `App`

None of them contain state, but `App` has to be a class-based component
so that we can use `componentDidMount`. We need it to attach D3 event
listeners.

### Particle

The `Particle` component is a circle. It looks like this:

``` javascript
// src/components/Particles/Particle.jsx
import React from 'react';

const Particle = ({ x, y }) => (
    <circle cx={x} cy={y} r="1.8" />
);

export default Particle;
```

It takes `x` and `y` coordinates and returns an SVG circle.

### Particles

The `Particles` component isn’t much smarter – it returns a list of
circles wrapped in a grouping element, like this:

``` javascript
// src/components/Particles/index.jsx
import React from 'react';
import Particle from './Particle';

const Particles = ({ particles }) => (
    <g>{particles.map(particle =>
        <Particle key={particle.id} {...particle} />
        )}
    </g>
);

export default Particles;
```

Walk through the array of particles, render a Particle component for
each. Declarative rendering that you’ve seen before :)

We can take an array of `{id, x, y}` objects and render SVG circles. Now
comes our first fun component: the `App`.

### App

`App` takes care of rendering the scene and attaching d3 event
listeners. It gets actions via props and ties them to mouse events. You
can think of actions as callbacks that work on the global data store
directly. No need to pass them through many levels of props.

The rendering part looks like this:

``` javascript
// src/components/index.jsx

import React, { Component } from 'react';
import { select as d3Select, mouse as d3Mouse, touches as d3Touches } from 'd3';

import Particles from './Particles';
import Footer from './Footer';
import Header from './Header';

class App extends Component {
  // ..
    render() {
        return (
            <div onMouseDown={e => this.props.startTicker()} style={{overflow: 'hidden'}}>
                 <Header />
                 <svg width={this.props.svgWidth}
                      height={this.props.svgHeight}
                      ref="svg"
                      style={{background: 'rgba(124, 224, 249, .3)'}}>
                     <Particles particles={this.props.particles} />
                 </svg>
                 <Footer N={this.props.particles.length} />
             </div>
        );
    }
}

export default App;
```

There’s more going on, but the gist is that we return a `<div>` with a
`Header`, a `Footer`, and an `<svg>`. Inside `<svg>`, we use `Particles`
to render many circles. The Header and Footer components are just some
helpful text.

Notice that the core of our rendering function only says *“Put all
Particles here, please”*. There’s nothing about what’s moved, what’s
new, or what’s no longer needed. We don’t have to worry about that.

We get a list of coordinates and naively render some circles. React
takes care of the rest.

Oh, and we call `startTicker()` when a user clicks on our scene. No
reason to have the clock running *before* any particles exist.

#### D3 event listeners

To let users generate particles, we have to wire up some functions in
`componentDidMount`. That looks like this:

``` javascript
// src/components/index.jsx

class App extends Component {
    svgWrap = React.createRef();
    
    componentDidMount() {
        let svg = d3Select(this.svgWrap.current);

        svg.on('mousedown', () => {
            this.updateMousePos();
            this.props.startParticles();
        });
        svg.on('touchstart', () => {
            this.updateTouchPos();
            this.props.startParticles();
        });
        svg.on('mousemove', () => {
            this.updateMousePos();
        });
        svg.on('touchmove', () => {
            this.updateTouchPos();
        });
        svg.on('mouseup', () => {
            this.props.stopParticles();
        });
        svg.on('touchend', () => {
            this.props.stopParticles();
        });
        svg.on('mouseleave', () => {
            this.props.stopParticles();
        });
    }

    updateMousePos() {
        let [x, y] = d3Mouse(this.svgWrap.current);
        this.props.updateMousePos(x, y);
    }

    updateTouchPos() {
        let [x, y] = d3Touches(this.svgWrap.current)[0];
        this.props.updateMousePos(x, y);
    }
```

There are several events we take into account:

  - `mousedown` and `touchstart` turn on particle generation
  - `mousemove` and `touchmove` update the mouse location
  - `mouseup`, `touchend`, and `mouseleave` turn off particle generation

Inside our event callbacks, we use `updateMousePos` and `updateTouchPos`
to update Redux state. They use `d3Mouse` and `d3Touches` to get `(x,
y)` coordinates for new particles relative to our SVG element and call
Redux actions passed-in via props. The particle generation step uses
this data as each particle’s initial position.

You’ll see that in the next section. I agree, it smells kind of
convoluted, but it’s for good reason: We need a reference to a mouse
event to get the cursor position, and we want to decouple particle
generation from event handling.

Remember, React isn’t smart enough to figure out mouse position relative
to our drawing area. React knows that we clicked a DOM node. [D3 does
some magic](https://github.com/d3/d3-selection/blob/master/src/mouse.js)
to find exact coordinates.

Touch events return lists of coordinates. One for each finger. We use
only the first coordinate because shooting particles out of multiple
fingers would make this example too hard.

That’s it for rendering and user events. [107 lines of
code](https://github.com/Swizec/react-particles-experiment/blob/svg-based-branch/src/components/index.jsx).
