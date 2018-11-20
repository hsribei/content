
## Step 4: CountyMap/County.js

The `County` component is built from two parts: imports and color
constants, and a component that returns a `<path>`. All the hard
calculation happens in `CountyMap`.

    // src/components/CountyMap/County.js
    import React, { Component } from 'react';
    import _ from 'lodash';
    
    const ChoroplethColors = _.reverse([
        'rgb(247,251,255)',
        'rgb(222,235,247)',
        'rgb(198,219,239)',
        'rgb(158,202,225)',
        'rgb(107,174,214)',
        'rgb(66,146,198)',
        'rgb(33,113,181)',
        'rgb(8,81,156)',
        'rgb(8,48,107)'
    ]);
    
    const BlankColor = 'rgb(240,240,240)'

We import React and Lodash, and define some color constants. I got the
`ChoroplethColors` from some example online, and `BlankColor` is a
pleasant gray.

Now we need the `County` component.

    // src/components/CountyMap/County.js
    class County extends Component {
        shouldComponentUpdate(nextProps, nextState) {
            const { zoom, value } = this.props;
    
            return zoom !== nextProps.zoom
                || value !== nextProps.value;
        }
    
        render() {
            const { value, geoPath, feature, quantize } = this.props;
    
            let color = BlankColor;
    
            if (value) {
                color = ChoroplethColors[quantize(value)];
            }
    
            return (
                <path d={geoPath(feature)}
                      style={{fill: color}}
                      title={feature.id} />
            );
        }
    }
    
    export default County;

The `render` method uses a `quantize` scale to pick the right color and
returns a `<path>` element. `geoPath` generates the `d` attribute, we
set style to `fill` the color, and we give our path a `title`.

`shouldComponentUpdate` is more interesting. It’s a React lifecycle
method that lets you specify which prop changes are relevant to
component re-rendering.

`CountyMap` passes complex props - `quantize`, `geoPath`, and `feature`.
They’re pass-by-reference instead of pass-by-value. That means React
can’t see when their output changes unless you make completely new
copies.

This can lead to all 3,220 counties re-rendering every time a user does
anything. But they only have to re-render if we change the map zoom or
if the county gets a new value.

Using `shouldComponentUpdate` like this we can go from 3,220 DOM updates
to a few hundred. Big speed improvement
