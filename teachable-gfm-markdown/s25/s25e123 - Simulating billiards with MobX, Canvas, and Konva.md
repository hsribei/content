
{\#billiards-simulation}

## Build a declarative billiards simulation with MobX, Canvas, and Konva

![Billiards
game](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/billiards-start.png)

We’re building a small game. You have 11 glass balls – marbles, if you
will. Grab one, throw it at the others, watch them bounce around. There
is no score, but it looks cool, and it’s a fun way to explore how Konva
and React give you interactive Canvas features.

We’re using React and Konva to render our 11 marbles on an HTML5 Canvas
element, MobX to drive the animation loop, and D3 to help with collision
detection. Because this example is declarative, we can split it into two
parts:

  - Part 1: Rendering the marbles
  - Part 2: Building the physics

You can see the finished [code on
Github](https://github.com/Swizec/declarative-canvas-react-konva) and
play around with a [hosted
version](https://swizec.github.io/declarative-canvas-react-konva/) of
the code you’re about to build.

I know this example comes late in the book, and you’re feeling like you
know all there is to React and visualizations. You can think of this
example as practice. Plus it’s a good way to learn the basics of MobX.
