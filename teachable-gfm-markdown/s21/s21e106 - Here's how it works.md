
## Here’s how it works

We use **React to render everything**: the page, the SVG element, the
particles inside. This lets us tap into React’s algorithms that decide
which nodes to update and when to garbage collect old nodes.

Then we use some **d3 calculations and event detection**. D3 has great
random generators, so we take advantage of that. D3’s mouse and touch
event handlers calculate coordinates relative to our SVG. We need those,
but React’s click handlers are based on DOM nodes, which don’t
correspond to `(x, y)` coordinates. D3 looks at real cursor position on
screen.

All **particle coordinates are in a Redux store**. Each particle also
has a movement vector. The store holds some useful flags and general
parameters, as well. This lets us treat animation as data
transformations. I’ll show you what that means.

We use **actions to communicate user events** like creating particles,
starting the animation, changing mouse position, and so on. On each
requestAnimationFrame, we **dispatch an “advance animation” action**.

On each action, the **reducer calculates a new state** for the whole
app. This includes **new particle positions** for each step of the
animation.

When the store updates, **React flushes changes** via props and because
**coordinates are state, the particles move**.

The result is smooth animation. Just like the game loop example from
before.
