
# Using a game loop for rich animation

Game loops are fun\! They’re my favorite. They even sound fun: “game
loop”. Doesn’t it sound fun to go build a game loop? Maybe it’s because
I’ve always used game loops when building something fun to play with.

At its core, a game loop is an infinite loop where each iteration
renders the next frame of your animation.

The concept comes from the video game industry. At some point they
realized that building game engines is much easier when you think about
each frame as its own static representation of your game state.

You take every object in the game and render it. Then you throw it all
away, make small adjustments, and render again.

This turns out to be both faster to run and easier to implement than
diffing scenes and figuring out what moves and what doesn’t. Of course
as you get more objects on the screen it becomes silly to re-render the
immovable background every time.

But you don’t have to worry about that. That’s React’s job.

React can figure out a diff between hierarchical representations of your
scene and re-render the appropriate objects.

That’s a hard engineering problem, but you can think of it this way:

1.  Render a DOM from state
2.  Change some values in state
3.  Trigger a re-render

Make those state changes fast enough and you get 60 frames per second.
The hard part then becomes thinking about your movement as snapshots in
time.

When something speeds up, what does that mean for changes in its
position?
