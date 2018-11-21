
### Step 1: stub out App and Ball

We start with a skeleton: An `App` component rendering a `BouncingBall`
component inside an SVG, and a `Ball` component.

``` javascript
// index.js
import React from "react";
import { render } from "react-dom";

import BouncingBall from "./BouncingBall";

const App = () => (
  <div>
    <svg width="800" height="600">
      <BouncingBall max_h={600} />
    </svg>
  </div>
);

render(<App />, document.getElementById("root"));
```

App imports dependencies, imports `BouncingBall`, renders it all into a
`root` DOM node. CodeSandbox gives us most of this code by default.

``` javascript
// Ball.js
import React from "react";

const Ball = ({ x, y }) => <circle cx={x} cy={y} r="5" />;

export default Ball;
```

We’re calling it a ball, but eh it’s a just a circle. You can make this
more fun if you want. Right now it’s a black SVG circle that renders at
a specified `x, y` coordinate with a radius of `5` pixels.

It’s these coordinates that we’re going to play with to make the ball
drop and bounce. Each time, React is going to re-render and move our
ball to its new coordinates. Because we change them so quickly, it looks
like the ball is animated. You’ll see.

-----

Those are the boring components. The animation game loop fun happens in
`BouncingBall`.
