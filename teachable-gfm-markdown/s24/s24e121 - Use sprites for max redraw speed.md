
### Using sprites for max redraw speed

Our [SVG-based Particles](#svg-particles) component was simple. Iterate
through a list of particles, render a `<Particle>` component for each.

We’re going to completely rewrite that. Our new approach goes like this:

0.  Cache a sprite on `componentDidMount`
1.  Clear canvas
2.  Redraw all particles
3.  Repeat

Because the new approach renders a flat image, and because we don’t care
about interaction with individual particles, we can get rid of the
`Particle` component. The unnecessary layer of nesting was slowing us
down.

The new `Particles` component looks like this:

``` javascript
// src/components/Particles.jsx

import React, { Component } from 'react';
import { FastLayer } from 'react-konva';

class Particles extends Component {
    layerRef = React.createRef();
    
    componentDidMount() {
        this.canvas = this.layerRef.current.canvas._canvas;
        this.canvasContext = this.canvas.getContext('2d');

        this.sprite = new Image();
        this.sprite.src = 'http://i.imgur.com/m5l6lhr.png';
    }

    drawParticle(particle) {
        let { x, y } = particle;

        this.canvasContext.drawImage(this.sprite, 0, 0, 128, 128, x, y, 15, 15);
    }

    componentDidUpdate() {
        let particles = this.props.particles;

        console.time('drawing');
        this.canvasContext.clearRect(0, 0, this.canvas.width, this.canvas.height);

        for (let i = 0; i < particles.length; i++) {
            this.drawParticle(particles[i]);
        }
        console.timeEnd('drawing');
    }

    render() {
        return (
            <FastLayer ref={this.layerRef} listening="false" />
        );
    }
}

export default Particles;
```

40 lines of code is a lot all at once. Let’s walk through step by step.

#### componentDidMount

``` javascript
// src/components/Particles.jsx

// ...
componentDidMount() {
    this.canvas = this.refs.layer.canvas._canvas;
    this.canvasContext = this.canvas.getContext('2d');

    this.sprite = new Image();
    this.sprite.src = 'http://i.imgur.com/m5l6lhr.png';
}
```

React calls `componentDidMount` when our component first renders. We use
it to set up 3 instance properties.

`this.canvas` is a reference to the HTML5 Canvas element. We get it
through a ref to the Konva layer, then spelunk through Konva internals
to get the canvas itself. As the `_` prefix indicates, Anton Lavrenov
did not intend this to be a public API.

Thanks to JavaScript’s permissiveness, we can use it anyway. 🙌

`this.canvasContext` is a reference to our canvas’s
[CanvasRenderingContext2D](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D).
It’s the interface we use to draw basic shapes, perform transformations,
and so on. Context is the only part of canvas you ever interact with as
a developer.

Why it’s not just Canvas, I don’t know.

`this.sprite` is a cached image. A small minion that we are going to
copy-paste all over as our particle. Creating a new image object with
`new Image()` and setting the `src` property downloads our sprite from
the internet into browser memory.

It looks like this:

![Our minion particle](http://i.imgur.com/m5l6lhr.png)

You might think it’s unsafe to copy references to rendered elements into
component properties like that, but it’s okay. Our render function
always renders the same thing, so the reference never changes. It just
makes our code cleaner.

Should our component unmount and re-mount, React will call
`componentDidMount` again and update our reference.

#### drawParticle

``` javascript
// src/components/Particles.jsx

// ...
drawParticle(particle) {
    let { x, y } = particle;

    this.canvasContext.drawImage(this.sprite, 0, 0, 128, 128, x, y, 15, 15);
}
```

`drawParticle` draws a single particle on the canvas. It gets
coordinates from the `particle` argument and uses `drawImage` to copy
our sprite into position.

We use the whole sprite, corner `(0, 0)` to corner `(128, 128)`. That’s
how big our sprite is. And we copy it to position `(x, y)` with a width
and height of `15` pixels.

`drawImage` is the fastest method I’ve found to put pixels on canvas. I
don’t know why it’s so fast, but here’s a [helpful
benchmark](https://jsperf.com/canvas-drawimage-vs-putimagedata/3) so you
can see for yourself.

#### componentDidUpdate

``` javascript
// src/components/Particles.jsx

// ...
componentDidUpdate() {
    let particles = this.props.particles;

    console.time('drawing');
    this.canvasContext.clearRect(0, 0, this.canvas.width, this.canvas.height);

    for (let i = 0; i < particles.length; i++) {
        this.drawParticle(particles[i]);
    }
    console.timeEnd('drawing');
}
```

`componentDidUpdate` is where the magic happens. React calls this
lifecycle method every time our list of particles changes. *After* the
`render` method.

Just like the D3 blackbox approach, we move rendering out of the
`render` method and into `componentDidUpdate`.

Here’s how it works:

1.  `this.canvasContext.clearRect` clears the entire canvas from
    coordinate `(0, 0)` to coordinate `(width, height)`. We delete
    everything and make the canvas transparent.
2.  We iterate our `particles` list with a `for` loop and call
    `drawParticle` on each element.

Clearing and redrawing the canvas is faster than moving individual
particles. For loops are faster than `.map` or any other form of
iteration. I tested. A lot.

Open your browser console and see how long each frame takes to draw. The
`console.time` - `console.timeEnd` pair measures how long it takes your
code to get from `time` to `timeEnd`. You can have as many of these
timers running as you want as long as you give them different names.

#### render

``` javascript
// src/components/Particles.jsx

// ...
render() {
    return (
        <FastLayer ref={this.layerRef} listening="false" />
    );
}
```

After all that work, our `render` method is quite short.

We render a Konva `FastLayer`, give it a `ref` and turn off `listening`
for mouse events. That makes the fast layer even faster.

Ideas for this combination of settings came from Konva’s official
[performance
tips](https://konvajs.github.io/docs/performance/All_Performance_Tips.html)
documentation. This makes sense when you think about it.

A `FastLayer` is faster than a `Layer`. It’s in the name. Ignoring mouse
events means you don’t have to keep track of elements. It reduces
computation.

This was empirically the fastest solution with the most particles on
screen.
