
# Using canvas

So far we’ve been rendering our visualizations with SVG. SVG is great
because it follows a familiar structure, offers infinitely scalable
vector graphics, and works everywhere. There are *some* advanced SVG
features you can’t use everywhere, but the core is solid.

However, SVG has a big flaw: it’s slow.

Anything more than a few hundred SVG nodes and your browser starts to
struggle. Especially if those thousands of elements move around.

A web animation panel moderator at ForwardJS once asked me, *“But why
would you want thousands of SVG elements?”*.

It was my first time participating in a panel, stage lights shining upon
me, a mildly disinterested audience staring into their phones … I
bombed: *“Errr … because you
can?”*.

![](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/stage_lights.jpg)

What I *should* have said was: *“Because there have been thousands of
UFO sightings, there are thousands of counties in the US, millions of
taxi rides, hundreds of millions of people having this or that
datapoint. And you want to show change over time.”*

That’s the real answer.

Sometimes, when you’re visualizing data, you have a lot of data. The
data changes over time. Animation is the best way to show change over
time.

Once upon a time, I worked on a [D3 video
course](https://www.packtpub.com/web-development/mastering-d3js-video)
for Packt and used UFO sightings as an example. At peak UFO sighting,
right before smartphones become a thing, the animation takes up to 2
seconds to redraw a single frame.

Terrible.

So if SVG is slow and you need to animate thousands of elements, what
are you to do? HTML5 Canvas.
