
### Step 3: Rendering

To render our Ball we have to tweak BouncingBall’s `render` method. A
small change. Try it yourself first.

``` javascript
// BouncingBall.js
import Ball from "./Ball"

class App extends Component {
    // ...
    render() {
    // render Ball at position y
    return <g><Ball x={10} y={this.state.y} /></g>
  }
}
```

Import our `Ball` component, render it at `x=10` and use `this.state.y`
for the vertical coordinate.

A black ball shows up on your screen. Like this:

![Black
ball](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/bouncing-ball.png)
