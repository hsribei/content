
### Ball.js

You’ve rendered a ball before: It’s an SVG circle. You can make it
fancier, but that’s not the point. The point are transitions.

The way D3 transitions work with React feels like combining the blackbox
and full integration approaches. Full integration for rendering, switch
to blackbox for the transition, release control back to React. 🤯

``` javascript
class Ball extends React.PureComponent {
  constructor(props) {
    super();
    this.state = {
      x: props.x
    };
  }

  circleRef = React.createRef();


  render() {
    const { x, y } = this.state;

    return <circle r="10" cx={x} cy={10} ref={this.circleRef} />;
  }
}
```

We render a `circle` at `x` coordinate and a random `y` and give it a
React ref. We’ll use this ref to give control of the DOM node to D3.
Just like blackbox components.

The interesting part is that we use a constructor to copy our `x` prop
into state. That’s because we’re going to use props as a sort of staging
area for what we *want* `x` to be, not what it currently is.

Props change, state stays the same. React doesn’t move our ball.
