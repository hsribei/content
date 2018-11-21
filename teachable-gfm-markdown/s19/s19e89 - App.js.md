
### App.js

Like we said: the hard stuff happens inside our Ball component. That
means App has render our Ball and implement a mechanism to toggle the
`x` coordinate between two states: Left and right.

``` javascript
class App extends React.Component {
  state = {
    ballLeft: true
  };
  ballJump = () =>
    this.setState({
      ballLeft: !this.state.ballLeft
    });

  render() {
    const { ballLeft } = this.state;
    return (
      <div style={styles}>
        <h1>D3 transitions in React 16.3 {"\u2728"}</h1>
        <p>Click the ball 👇</p>
        <svg style={{ width: "300", height: "300px" }} onClick={this.ballJump}>
          <Ball x={ballLeft ? 15 : 250} />
        </svg>
      </div>
    );
  }
}
```

Typical class based component:

1.  State holding a `ballLeft` boolean
2.  A method to toggle said boolean
3.  A render method that renders an SVG with an `onClick` event

When `state.ballLeft` is true, ball renders on the left. When it’s
false, on the right. Left and right are hardcoded `x` coordinates.

Ball component handles the rest.
