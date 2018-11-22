
### Preparing a canvas layer

Our changes start in `src/components/index.jsx`. We have to throw away
the `<svg>` element and replace it with a Konva stage.

You can think of a Konva stage as a Canvas element with a bunch of
helper methods attached. Some of them Konva uses internally; others are
exposed as an API. Functions like exporting to an image file, detecting
intersections, etc.

{caption: “Import Konva and set the stage”, line-numbers: false}

``` javascript
// src/components/index.jsx

// ...
import { Stage } from 'react-konva';

// ...
class App extends Component {
    // ..
    render() {
        return (
            // ..
                     <Stage width={this.props.svgWidth} height={this.props.svgHeight}>
                         <Particles particles={this.props.particles} />

                     </Stage>

                 </div>
                 <Footer N={this.props.particles.length} />
             </div>
        );
    }
}
```

We import `Stage` from `react-konva`, then use it instead of the `<svg>`
element in the `render` method. It gets a `width` and a `height`.

Inside, we render the `Particles` component. It’s going to create a
Konva layer and use low-level Canvas methods to render particles as
sprites.
