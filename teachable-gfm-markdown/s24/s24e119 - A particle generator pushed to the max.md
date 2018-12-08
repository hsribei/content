
## A particle generator pushed to 20,000 elements with Canvas

Our [SVG-based particle
generator](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6906668#animating-react-redux)
caps out at a few thousand elements. Animation becomes slow as times
between iterations of our game loop increase.

Old elements leave the screen and get pruned faster than we can create
new ones. This creates a natural upper limit to how many elements we can
push into the page.

We can render many more elements if we take out SVG and use HTML5 Canvas
instead. I was able to push the code up to almost 20,000 smoothly
animated elements. Then JavaScript became the bottleneck.

Well, I say JavaScript was the bottleneck, but monitor size plays a role
too. It goes up to 20,000 on my laptop screen, juuuust grazes 30,000 on
my large desktop monitor, and averages about 17,000 on my iPhone 5SE.

Friends with newer laptops got it up to 35,000.

You can see it in action hosted on [Github
pages](http://swizec.github.io/react-particles-experiment/).

We’re keeping most of our existing code. The real changes happen in
`src/components/index.jsx`, where a Konva stage replaces the `<svg>`
element, and in `src/components/Particles.jsx`, where we change what we
render. There’s a small tweak in the reducer to generate more particles
per tick.

You should go into your particle generator directory, install Konva and
react-konva, and then make the changes below. Trying things out is
better than just reading my code ;)

    $ npm install --save konva react-konva

{aside} react-konva is a thin wrapper on Konva itself. There’s no need
to think about it as its own thing. For the most part, you can go into
the Konva docs, read about something, and it Just Works™ in react-konva.
{/aside}
