
## Step 3: CountyMap/CountyMap.js

Here comes the fun part - declaratively drawing a map. You’ll see why I
love using React for dataviz.

We’re using the [full-feature integration](#full-feature-integration)
approach and a lot of D3 maps magic. Drawing a map with D3 I’m always
surprised how little code it takes.

Start with the imports: React, D3, lodash, topojson, County component.

    // src/components/CountyMap/CountyMap.js
    import React, { Component } from 'react';
    import * as d3 from 'd3';
    import * as topojson from 'topojson';
    import _ from 'lodash';
    
    import County from './County';

Out of these, we haven’t built `County` yet, and you haven’t seen
`topojson` before.

TopoJSON is a geographical data format based on JSON. We’re using the
`topojson` library to translate our geographical datasets into GeoJSON,
which is another way of defining geo data with JSON.

I don’t know why there are two, but TopoJSON produces smaller files, and
GeoJSON can be fed directly into D3’s geo functions. ¯\\*(ツ)*/¯

Maybe it’s a case of [competing standards](https://xkcd.com/927/).

### Constructor

We stub out the `CountyMap` component then fill it in with logic.

{format: javascript, line-numbers: false, caption: “CountyMap stub”}

    // src/components/CountyMap/CountyMap.js
    class CountyMap extends Component {
        constructor(props) {
            super(props);
    
            this.state = {
            }
        }
    
        static getDerivedStateFromProps(props, state) {
    
        }
    
        render() {
            const { usTopoJson } = this.props;
    
            if (!usTopoJson) {
                return null;
            }else{
                return (
    
                );
            }
        }
    }
    
    export default CountyMap;

We’ll set up default D3 state in `constructor` and keep it up to date
with `getDerivedStateFromProps`.

We need three D3 objects to build a choropleth map: a geographical
projection, a path generator, and a quantize scale for colors.

{format: javascript, line-numbers: false, caption: “D3 objects for a
map”}

    // src/components/CountyMap/CountyMap.js
    class CountyMap extends React.Component {
        constructor(props) {
            super(props);
    
            const projection = d3.geoAlbersUsa().scale(1280);
    
            this.state = {
                geoPath: d3.geoPath().projection(projection),
                quantize: d3.scaleQuantize().range(d3.range(9)),
                projection
            };
        }

You might remember geographical projections from high school. They map a
sphere to a flat surface. We use `geoAlbersUsa` because it’s made
specifically for maps of the USA.

D3 offers many other projections. You can see them on [d3-geo’s Github
page](https://github.com/d3/d3-geo#projections).

A `geoPath` generator takes a projection and returns a function that
generates the `d` attribute of `<path>` elements. This is the most
general way to specify SVG shapes. I won’t go into explaining the `d`
here, but it’s [an entire
language](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/d)
for describing shapes.

`quantize` is a D3 scale. We’ve talked about the basics of scales in the
[D3 Axis example](#blackbox-axis). This one splits a domain into 9
quantiles and assigns them specific values from the `range`.

Let’s say our domain goes from 0 to 90. Calling the scale with any
number between 0 and 9 would return 1. 10 to 19 returns 2 and so on.
We’ll use it to pick colors from an array.

### getDerivedStateFromProps

Keeping our geo path and quantize scale up to date is simple, but we’ll
make it harder by adding a zoom feature. It won’t work until we build
the filtering, but hey, we’ll already have it by then\! :D

{format: javascript, line-numbers: false, caption: “CountyMap
getDerivedStateFromProps”}

``` 
        // src/components/CountyMap/CountyMap.js
    static getDerivedStateFromProps(props, state) {
        let { projection, quantize, geoPath } = state;

        projection
            .translate([props.width / 2, props.height / 2])
            .scale(props.width * 1.3);

        if (props.zoom && props.usTopoJson) {
            const us = props.usTopoJson,
                USstatePaths = topojson.feature(us, us.objects.states).features,
                id = _.find(props.USstateNames, { code: props.zoom }).id;

            projection.scale(props.width * 4.5);

            const centroid = geoPath.centroid(_.find(USstatePaths, { id: id })),
                translate = projection.translate();

            projection.translate([
                translate[0] - centroid[0] + props.width / 2,
                translate[1] - centroid[1] + props.height / 2
            ]);
        }

        if (props.values) {
            quantize.domain([
                d3.quantile(props.values, 0.15, d => d.value),
                d3.quantile(props.values, 0.85, d => d.value)
            ]);
        }

        return {
            ...state,
            projection,
            quantize
        };
    }
```

There’s a lot going on here.

We destructure `projection`, `quantize`, and `geoPath` out of component
state. These are the D3 object we’re about to update.

First up is the projection. We translate (move) it to the center of our
drawing area and set the scale property. You have to play around with
this value until you get a nice result because it’s different for every
projection.

Then we do some weird stuff if `zoom` is defined.

We get the list of all US state features in our geo data, find the one
we’re `zoom`-ing on, and use the `geoPath.centroid` method to calculate
its center point. This gives us a new coordinate to `translate` our
projection onto.

The calculation in `.translate()` helps us align the center point of our
`zoom` US state with the center of the drawing area.

While all of this is going on, we also tweak the `.scale` property to
make the map bigger. This creates a zooming effect.

After all that, we update the quantize scale’s domain with new values.
Using `d3.quantile` lets us offset the scale to produce a more
interesting map. Again, I discovered these values through experiment -
they cut off the top and bottom of the range because there isn’t much
there. This brings higher contrast to the richer middle of the range.

With our D3 objects updated, we return a new derived state.
*Technically* you don’t have to do this but you should. Due to how
JavaScript works, you already updated D3 objects in place, but you
should pretend `this.state` is immutable and return a new copy.

Overall that makes your code easier to understand and closer to React
principles.

### render

After all that hard work, the `render` method is a breeze. We prep our
data then loop through it and render a `County` element for each entry.

{format: javascript, line-numbers: false, caption: “CountyMap render”}

``` 
    render() {
        const { geoPath, quantize } = this.state,
            { usTopoJson, values, zoom } = this.props;

        if (!usTopoJson) {
            return null;
        } else {
            const us = usTopoJson,
                USstatesMesh = topojson.mesh(
                    us,
                    us.objects.states,
                    (a, b) => a !== b
                ),
                counties = topojson.feature(us, us.objects.counties).features;

            const countyValueMap = _.fromPairs(
                values.map(d => [d.countyID, d.value])
            );

            return (
                <g>
                    {counties.map(feature => (
                        <County
                            geoPath={geoPath}
                            feature={feature}
                            zoom={zoom}
                            key={feature.id}
                            quantize={quantize}
                            value={countyValueMap[feature.id]}
                        />
                    ))}

                    <path
                        d={geoPath(USstatesMesh)}
                        style={{
                            fill: "none",
                            stroke: "#fff",
                            strokeLinejoin: "round"
                        }}
                    />
                </g>
            );
        }
    }
```

We use destructuring to save all relevant props and state in constants,
then use the TopoJSON library to grab data out of the `usTopoJson`
dataset.

`.mesh` calculates a mesh for US states – a thin line around the edges.
`.feature` calculates feature for each count – fill in with color.

Mesh and feature aren’t tied to US states or counties by the way. It’s
just a matter of what you get back: borders or flat areas. What you need
depends on what you’re building.

We use Lodash’s `_.fromPairs` to build a dictionary that maps county
identifiers to their values. Building it beforehand makes our code
faster. You can read some details about the speed optimizations
[here](https://swizec.com/blog/optimizing-react-choropleth-map-rendering/swizec/7302).

As promised, the `return` statement loops through the list of `counties`
and renders `County` components. Each gets a bunch of attributes and
returns a `<path>` element that looks like a specific county.

For US state borders, we render a single `<path>` element and use
`geoPath` to generate the `d` attribute.
