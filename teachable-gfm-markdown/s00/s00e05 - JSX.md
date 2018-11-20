
# JSX

Our components are going to use JSX, a JavaScript syntax extension that
lets us treat XML-like data as normal code. You can use React without
JSX, but JSX makes React’s full power easier to use.

The gist of JSX is that we can use any XML-like string just like it was
part of JavaScript. No Mustache or messy string concatenation necessary.
Your functions can return HTML, SVG, or XML.

For instance, code that renders one of our first examples – a color
swatch – looks like this:

``` javascript
ReactDOM.render(
    <Colors width="400" />, 
    document.getElementById('svg')
);
```

Which compiles to:

``` javascript
ReactDOM.render(
    React.createElement(Colors, {width: "400"}),
    document.querySelectorAll('svg')[0]
);
```

HTML code translates to `React.createElement` calls with attributes
translated into a property dictionary. The beauty of this approach is
two-pronged: you can use React components as if they were HTML tags, and
HTML attributes can be anything.

You’ll see that anything from a simple value to a function or an object
works equally well.
