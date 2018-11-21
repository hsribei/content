
# Speed optimizations

Welcome to the speed optimization chapter. This is where we make our
code harder to read and faster to run.

You might never need any techniques discussed here.

You already know how to build performant data visualization components.
For 99% of applications, plain code that’s easy to read and understand
is faster than fast code that’s hard to read.

You and your team spend most of your time reading code. Optimize for
that. The faster you can code, the faster your system can evolve. Leave
runtime optimization to React and its ecosystem of library authors for
as long as you can get away with.

Do you really need to save that tenth of a second at runtime if it means
an extra hour of head scratching every time there’s a bug?

Be honest. 😉

That said, there *are* cases where faster code is also easier to read.
And there are cases where your visualization is so massive, that you
need every ounce of oomph you can get.

For the most part, we’re going to talk about three things:

  - using Canvas to speed up rendering
  - using React-like libraries to speed up the core rendering engine
  - avoiding unnecessary computation and redraws
  - reaching for WebGL when even Canvas isn’t fast enough

We’ll start with Canvas because it’s the best bang for buck improvement
you can make.
