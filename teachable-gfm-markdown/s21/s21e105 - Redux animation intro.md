
# Animating with React, Redux, and d3

And now for some pure nerdy fun: A particle generator… or, well, as
close as you can get with React and D3. You’d need WebGL for a *real*
particle generator.

We’re making tiny circles fly out of your mouse cursor. Works on mobile
with your finger, too.

To see the particle generator in action, [go
here](http://swizec.github.io/react-particles-experiment/). Github won’t
let me host different branches, so you’ll see the advanced 20,000
particle version from next chapter.

We’re using the [game
loop](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6906604#game-loop)
approach to animation and Redux to store the state tree and drive
changes for each frame.

You can see the full code [on
GitHub](https://github.com/Swizec/react-particles-experiment). I’ve
merged the SVG and Canvas branches. Redux part is the same, some
parameters differ. We’re focusing on the Redux part because that’s
what’s new.

It’s going to be great.

{aside} Code in this example uses the `.jsx` file extension. I
originally wrote it back when that was still a thing, and while I did
update everything to React 16+, I felt that changing filenames was
unnecessary. {/aside}
