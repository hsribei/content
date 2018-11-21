
## The trouble with HTML5 Canvas

The tricky thing with HTML5 Canvas is that the API is low level and that
canvas is flat. As far as your JavaScript and React code are concerned,
it’s a flat image. It could be anything.

The lack of structure makes it difficult to detect clicks on elements,
interactions between shapes, when something covers something else, how
the user interacts with your stuff and so on Anything that requires
understanding what’s rendered.

You have to move most of that logic into your data store and manually
keep track.

As you can imagine, this becomes cumbersome. And you still can’t detect
user interaction because all you get is *“User clicked on coordinate (x,
y). Have fun.”*

At the same time, the low level API makes abstractions difficult. You
can’t create components for “this is a map” or “histogram goes here”.
You’re always down to circles and rectangles and basic shapes.

Your code soon looks like the D3.js spaghetti we tried to avoid in the
first
place.
