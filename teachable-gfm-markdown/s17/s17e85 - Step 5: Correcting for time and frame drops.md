
### Step 5: Correcting for time and frame drops

If you run the CodeSandbox a few times, you’ll notice two bugs.

1)  Sometimes our ball gets trapped at the bottom of the bounce. We
    won’t fix this one; it’s tricky.

2)  When you slow down your computer, the ball lags. That’s not how
    things behave in real life.

We’re dealing with dropped frames.

Modern browsers slow down JavaScript in tabs that aren’t focused, on
computers running off batteries, when batteries get low… many different
reasons. To make our animation silky smooth, we have to take this into
account.

That involves calculating how many frames we dropped and adjusting our
physics.

``` javascript
// BouncingBall.js
  gameLoop = () => {
    let { y, vy, lastFrame } = this.state;

    let frames = 1;

    if (lastFrame) {
      frames = (d3.now() - lastFrame) / (1000 / 60);
    }

    for (let i = 0; i < frames; i++) {
      if (y > this.props.max_h) {
        vy = -vy * 0.87;
      }

      y = y + vy;
      vy = vy + 0.3;
    }

    this.setState({
      y,
      vy,
      lastFrame: d3.now()
    });
  };
```

We add `lastFrame` to game state and set it with `d3.now()`. This gives
us a high resolution timestamp that’s pegged to `requestAnimationFrame`.
D3 guarantees that every `d3.now()` called within the same frame gets
the same timestamp.

`(d3.now()-lastFrame)/16` tells us how many frames were meant to have
happened since last iteration. Most of the time, this value will be `1`.

We use `frames` to simulate as many frames as we need in a quick loop.
Run the same calculations with the same height checks. This simulates
the missing frames and makes our animation look smooth.

Try it out on CodeSandbox: [click me for time-fixed bouncy
ball](https://codesandbox.io/s/8n3p6720wl)
