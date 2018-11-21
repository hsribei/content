
### Step 2: Stub out BouncingBall

We encapsulate everything in this component so that `App` doesn’t have
to know about the details of our animation. App declaratively renders a
bouncing ball and that’s it.

``` javascript
import React, { Component } from "react";
import * as d3 from "d3";

class BouncingBall extends Component {
  constructor() {
    super();

    this.state = {
      y: 5,
      vy: 0
    };
  }

  componentDidMount() {
    // start game loop
    this.timer = d3.timer(this.gameLoop);
  }

  componentWillUnmount() {
    // stop loop
    this.timer.stop();
  }

  gameLoop = () => {
    // move ball by vy
    // increase vy to accelerate
  }

  render() {
    // render Ball at position y
    return <g />;
  }
}

export default BouncingBall;
```

Nothing renders yet and we’re set up to get started.

We start with default state: vertical position `y=5`, vertical speed
`vy=0`.

`componentDidMount` fires off a d3 timer and `componentWillUnmount`
stops it so we don’t have unwanted code running in the background. The
timer calls `this.gameLoop` every 16 milliseconds, which translates to
about 60 frames per second.

D3 timers work like JavaScript’s own `setInterval`, but their
implementation is more robust. They use `requestAnimationFrame` to save
computer resources when possible, and default to `setInterval`
otherwise.

Unlike `setInterval` timers are also synced so their frames match up,
can be restarted, stopped, etc. You should default to favoring
`d3.timer` over `setInterval` or hacking your own
`requestAnimationFrame` implementation.
