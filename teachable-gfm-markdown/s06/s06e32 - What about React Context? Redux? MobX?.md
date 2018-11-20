
## What about React Context? Redux? MobX?

You may have heard of React Context, Redux, MobX and other state
handling libraries. They’re all great in different ways and the internet
can’t decide which one is best. Everyone has their own little twist on
the story.

And yet the basic principles are all the same:

1.  Single source of truth
2.  State flows down
3.  Updates flow up

Where React Context, Redux, MobX and other libraries help, is how much
work it is to build this machinery and keep it running. How much
flexibility you get when moving components. Basically how easy it is to
change your app later.

Remember the rewrite conundrum?

Our basic approach binds business structure to UI structure. Your state,
your props, your callbacks, they all follow the same hierarchy as your
UI does.

Want to move the buy button? Great\! You have to update the entire chain
of components leading from your state to that button.

Everything needs new callbacks and new props.

This is known as prop drilling and fast becomes super tedious. Rewiring
your whole app just to move a single button is no fun.

To solve this problem, React Context, Redux, MobX, etc. decouple your
business logic from your UI architecture. Take state out of the main
component and move it into its own object. Connect everything to that
instead.

\[picture\]

Now it doesn’t matter where you move that button. It still triggers the
same function on the same state. Every other component that cares about
that state updates too.

Different libraries have different details for how that works, but they
all follow the same idea and solve the same problem.

-----

We’re sticking with the basic approach because it’s easier to explain,
works without additional libraries, and is Good Enough™.

You can see an approach to using Redux in dataviz in the [Animating with
React, Redux, and D3 chapter](#animating-react-redux), and we tackle
MobX in the [MobX chapter](#refactoring-to-mobx).
