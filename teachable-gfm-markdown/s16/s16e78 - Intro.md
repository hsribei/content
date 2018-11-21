
# Animation

Welcome to the animation section. This is where the real fun begins.
Demos that look cool, impress your friends, and sound good at the dinner
table.

At my dinner table at least
…

![](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/puking-rainbows.png)

You already know how React and D3 work together, so these examples are
going to go faster. You know that we’re using React for rendering SVG
and D3 for calculating props. You know how to make your dataviz
interactive and how to handle oodles of data.

Now you’re going to learn how to make it dance. Smooth transitions
between states, complex animations, interacting with users in real-time.
60 frames per second, baby\!

Animation looks like this in a nutshell:

  - render something
  - change it 60 times per second

When that happens humans perceive smooth motion. You can go as low as 24
frames per second, like old TVs used to, but that often looks jaggeddy
on modern monitors.

You’re about to learn two different approaches to building these
animations. Using game loops and using transitions.

1.  Game loops give you more control
2.  Transitions are easier to implement

We’re starting with an example or two in CodeSandbox, then building some
bigger stuff. No more big huge projects like the [tech salary
visualization](#salary-visualization), though. Takes too long to set up
and doesn’t focus on animation.
