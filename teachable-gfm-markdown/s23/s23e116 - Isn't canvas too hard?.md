
## Isn’t that too hard?

You might think all this pixel stuff sounds complicated. We’ve stayed in
shape and component land so far. We didn’t care about any pixels or
rendering details. Draw a rectangle and a wild rectangle appears.

How do you know which pixel should do what when you render with Canvas?

HTML5 Canvas does offer some shape primitives. It has circles and
rectangles and things like that, but they suffer from the same problem
that SVG does. The browser has to use your CPU to calculate those, and
at around 10,000 elements, things break down.

10,000 elements is still a hell of a lot more than the 3,000 or so that
SVG gives you.

If your app allows it, you can use sprites: Tiny images copy-pasted on
the Canvas as bytestreams. I have yet to find an upper bound for those.
My JavaScript became the bottleneck :D

But I’m getting ahead of myself. We’ll talk about sprites later.
