
### But why, Swizec?

I’m glad you asked. This was a silly example. I devised the experiment
because at my first React+D3 workshop somebody asked, *“What if we have
thousands of datapoints, and we want to animate all of them?”*. I didn’t
have a good answer.

Now I do. You put them in Canvas. You drive the animation with a game
loop. You’re good.

You can even do it as an overlay. Have an SVG for your graphs and
charts, overlay with a transparent canvas for your high speed
animation.
