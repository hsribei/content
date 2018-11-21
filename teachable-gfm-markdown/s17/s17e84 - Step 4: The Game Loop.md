
### Step 4: The Game Loop

Our game loop is already running. It’s that `d3.timer` we started on
component mount.

What do you think we should do every time `gameLoop` is called?

Move the ball. Accelerate. Bounce it back. 🏀

The physics is tricky to think about. Few students at my workshops
figure this one out and that’s the point: Game loop gives you control.
All the control.

``` javascript
// BouncingBall.js
    componentDidMount() {
        // start game loop
        this.timer = d3.timer(this.gameLoop);
    }
    
    componentWillUnmount() {
        this.timer.stop();
    }
    
    gameLoop = () => {
    let { y, vy } = this.state;

    if (y > this.props.max_h) {
      vy = -vy * .87;
    }

    this.setState({
      y: y + vy,
      vy: vy + 0.3
    })
  }
```

Our `gameLoop` method is called every 16 milliseconds give or take.
Sometimes more, if the computer is busy, low on batter, or the tab is
left inactive for too long

Bounce physics go like this:

  - add vertical speed, `vy`, to position, `y`
  - increased `vy` by amount of acceleration
  - if position is more than max, bounce
  - bounce means invert speed
  - bounce means energy loss, reduce speed by 13%

High school physics my friend. You might not remember.

Speed tells you how much an object moves per unit of time. Our speed
starts at 5 pixels per frame.

Acceleration tells you how much speed increases per unit of time.
Gravity makes you fall 10 meters per second faster every second. Our
gravity looks best at 0.3 pixels per frame.

Bounce is what really breaks people’s brains.

I think it’s one of Newton’s laws? Or a derivation from there. Bouncing
means balls maintain the same speed, but change direction. Our wall is
perpendicular to the path of travel, so the speed inverts.

We add some energy loss to make the bounce more realistic.

Gravity acceleration points the same downwards direction as always.
Except now it’s slowing down the ball instead of speeding it up.

Eventually speed reaches zero and the ball starts falling again.

![Magic](https://media.giphy.com/media/12NUbkX6p4xOO4/giphy.gif)

Here’s a CodeSandbox with the [final bouncy ball
code](https://codesandbox.io/s/o4vk8zkn96).

Experiment with multiple balls. Start them at different heights. Play
with different factors in those equations. It’s fun if you’re a nerd
like
me.
