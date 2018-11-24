
# A D3 blackbox higher order component – HOC

After that example you might think this is hella tedious to implement
every time. You’d be right\!

Good thing you can abstract it all away with a higher order component
– a HOC. Now this is something I should open source (just do it
already), but I want to show you how it works so you can learn about the
HOC pattern.

Higher order components are great when you see multiple React components
sharing similar code. In our case, that shared code is:

  - rendering an anchor element
  - calling D3’s render on updates

With a HOC, we can abstract that away into a sort of [object
factory](https://en.wikipedia.org/wiki/Factory_method_pattern). It’s an
old concept making a comeback now that JavaScript has classes.

Think of our HOC as a function that takes some params and creates a
class – a React component. Another way to think about HOCs is that
they’re React components wrapping other React components and a
function that makes it easy.

A HOC for D3 blackbox integration, called `D3blackbox`, looks like like
this:

``` javascript
function D3blackbox(D3render) {
    return class Blackbox extends React.Component {
        anchor = React.createRef();
        
        componentDidMount() { D3render.call(this); }
        componentDidUpdate() { D3render.call(this) }

        render() {
        const { x, y } = this.props;
        return <g transform={`translate(${x}, ${y})`} ref={this.anchor} />;
        }
    }
}
```

You’ll recognize most of that code from earlier.

We have `componentDidMount` and`componentDidUpdate` lifecycle hooks that
call `D3render` on component updates. `render` renders a grouping
element as an anchor with a ref so D3 can use it to render stuff into.

Because `D3render` is no longer a part of our component, we have to use
`.call` to give it the scope we want: this class, or rather `this`
instance of the `Blackbox` class.

We’ve also made some changes that make `render` more flexible. Instead
of hardcoding the `translate()` transformation, we take `x` and `y`
props. `{ x, y } = this.props` takes `x` and `y` out of `this.props`
using object decomposition, and we used ES6 string templates for the
`transform` attribute.

Consult the [ES6 cheatsheet](https://es6cheatsheet.com/) for details on
the syntax.

Using our new `D3blackbox` HOC to make an axis looks like this:

``` javascript
const Axis = D3blackbox(function () {
    const scale = d3.scaleLinear()
                .domain([0, 10])
                .range([0, 200]);
    const axis = d3.axisBottom(scale);

    d3.select(this.anchor)
      .call(axis);    
});
```

You know this code\! We copy pasted our axis rendering code from before,
wrapped it in a function, and passed it into `D3blackbox`. Now it’s a
React component.

Play with this example on [Codesandbox,
here](https://codesandbox.io/s/5v21r0wo4x).
