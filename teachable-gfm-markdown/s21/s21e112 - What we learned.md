
## What we learned

Building a particle generator in React and Redux, we made three
important discoveries:

1.  **Redux is much faster than you’d think**. Creating a new copy of
    the state tree on each animation loop sounds crazy, but it works.
    Most of our code creates shallow copies, which explains the speed.
2.  **Adding to JavaScript arrays is slow**. Once we hit about 300
    particles, adding new ones becomes slow. Stop adding particles and
    you get smooth animation. This indicates that something about
    creating particles is slow: either adding to the array, or creating
    React component instances, or creating SVG DOM nodes.
3.  **SVG is also slow**. To test the above hypothesis, I made the
    generator create 3000 particles on first click. The animation speed
    is *terrible* at first and becomes okayish at around 1000 particles.
    This suggests that making shallow copies of big arrays and moving
    existing SVG nodes around is faster than adding new DOM nodes and
    array elements. [Here’s a gif](http://i.imgur.com/ug478Me.gif)

-----

There you go: Animating with React, Redux, and D3. Kind of a new
superpower 😉

Here’s the recap:

  - React handles rendering
  - D3 calculates stuff, detects mouse positions
  - Redux handles state
  - element coordinates are state
  - change coordinates on every `requestAnimationFrame`
  - animation\!

Now let’s render to canvas and push this sucker to 20,000 smoothly
animated elements. Even on a mobile phone.
