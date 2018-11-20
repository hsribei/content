
# How React makes D3 easier

Our visualizations are going to use SVG – an XML-based image format that
lets us describe images in terms of mathematical shapes. For example,
the source code of an 800x600 SVG image with a rectangle looks like
this:

``` html
<svg width="800" height="600">
    <rect width="100" height="200" x="50" y="20" />
</svg>
```

These four lines create an SVG image with a black rectangle at
coordinates `(50, 20)` that is 100x200 pixels large. Black fill with no
borders is default for SVG shapes.

SVG is perfect for data visualization on the web because it works in all
browsers, renders without blurring or artifacts on all screens, and
supports animation and user interaction. You can see examples of
interaction and animation later in this book.

But SVG can get slow when you have many thousands of elements on screen.
We’re going to solve that problem by rendering bitmap images with
canvas. More on that later.

-----

Another nice feature of SVG is that it’s just a dialect of XML - nested
elements describe structure, attributes describe the details. Same
principles that HTML uses.

That makes React’s rendering engine particularly suited for SVG. Our
100x200 rectangle from before looks like this as a React component:

    const Rectangle = () => (
        <rect width="100" height="200" x="50" y="20" />
    );

To use this rectangle component in a picture, you’d use a component like
this:

    const Picture = () => (
        <svg width="800" height="600">
            <Rectangle />
        </svg>
    );

Sure looks like tons of work for a static rectangle. But look closely\!
Even if you know nothing about React and JSX, you can look at that code
and see that it’s a `Picture` of a `Rectangle`.

Compare that to a pure D3 approach:

    d3.select("svg")
      .attr("width", 800)
      .attr("height", 600)
      .append("rect")
      .attr("width", 100)
      .attr("height", 200)
      .attr("x", 50)
      .attr("y", 20);

It’s elegant, it’s declarative, and it looks like function call soup. It
doesn’t scream *“Rectangle in an SVG”* to as much as the React version
does.

You have to take your time and read the code carefully: first, we
`select` the `svg` element, then we add attributes for `width` and
`height`. After that, we `append` a `rect` element and set its
attributes for `width`, `height`, `x`, and `y`.

Those 8 lines of code create HTML that looks like this:

    <svg width="800" height="600">
        <rect width="100" height="200" x="50" y="20" />
    </svg>

Would’ve been easier to just write the HTML, right? Yes, for static
images, you’re better off using Photoshop or Sketch then exporting to
SVG.

Dealing with the DOM is not D3’s strong suit. There’s a lot of typing,
code that’s hard to read, it’s slow when you have thousands of elements,
and it’s often hard to keep track of which elements you’re changing.
D3’s enter-update-exit cycle is great in theory, but most people
struggle trying to wrap their head around it.

If you don’t understand what I just said, don’t worry. We’ll cover the
enter-update-exit cycle in the animations example.

Don’t worry about D3 either. **It’s hard\!** I’ve written two books
about D3, and I still spend as much time reading the docs as writing the
code. The library is huge and there’s much to learn. I’ll explain
everything as we go along.

D3’s strong suit is its ability to do everything except the DOM. There
are statistical functions, great support for data manipulation, a bunch
of built-in data visualizations, magic around transitions and animation
… **D3 can calculate anything for you. All you have to do is draw it
out.**

That’s why our general approach sounds like this in a nutshell:

  - React owns the DOM
  - D3 calculates properties

We leverage React for SVG structure and rendering optimizations; D3 for
its mathematical and visualization functions.

Now let’s look at three different ways of using React and D3 to build
data visualization:

  - using a library
  - quick blackbox components
  - full feature integration
