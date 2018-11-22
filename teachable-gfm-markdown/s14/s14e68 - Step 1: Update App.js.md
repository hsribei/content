
## Step 1: Update App.js

All right, you know the drill. Add imports, tweak some things, add to
render. We have to import `Controls`, set up filtering, update the map’s
`zoom` prop, and render a white rectangle and `Controls`.

The white rectangle makes it so the zoomed-in map doesn’t cover up the
histogram. I’ll explain when we get there.

    // src/App.js
    import MedianLine from './components/MedianLine';
    
    // markua-start-insert
    import Controls from './components/Controls';
    // markua-end-insert
    
    class App extends React.Component {
        state = {
            // ...
            medianIncomes: [],
            // markua-start-insert
            salariesFilter: () => true,
            // markua-end-insert
            filteredBy: {
                // ...
            }
        }
    
        // ...
    
        // markua-start-insert
        updateDataFilter = (filter, filteredBy) => {
            this.setState({
                salariesFilter: filter,
                filteredBy: filteredBy
            });
        }
        // markua-end-insert
    
        render() {
            // ...
        }
    }

We import the `Controls` component and add a default `salariesFilter`
function to `this.state`. The `updateDataFilter` method passes the
filter function and `filteredBy` dictionary from arguments to App state.
We’ll use it as a callback in `Controls`.

The rest of filtering setup happens in the render method.

    // src/App.js
    class App extends React.Component {
        // ...
    
        render() {
            // ...
            // markua-start-delete
            const filteredSalaries = techSalaries
            // markua-end-delete
            // markua-start-insert
            const filteredSalaries = techSalaries
                                         .filter(this.state.salariesFilter)
            // markua-end-insert
    
            // ...
    
            let zoom = null,
                medianHousehold = // ...
            // markua-start-insert
            if (filteredBy.USstate !== '*') {
                zoom = this.state.filteredBy.USstate;
                medianHousehold = d3.mean(medianIncomesByUSState[zoom],
                                          d => d.medianIncome);
            }
            // markua-end-insert
    
            // ...
        }
    }

We add a `.filter` call to `filteredSalaries`, which uses our
`salariesFilter` method to throw out anything that doesn’t fit. Then we
set up `zoom`, if a US state was selected.

We built the `CountyMap` component to focus on a given US state. Finding
the centroid of a polygon, re-centering the map, and increasing the
sizing factor. It creates a nice zoom effect.

![Zoom
effect](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/zoom-effect.png)

And here’s the downside of this approach. SVG doesn’t know about element
boundaries. It just renders stuff.

![Zoom without white
rectangle](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/zoom-without-rectangle.png)

See, it goes under the histogram. Let’s fix that and add the `Controls`
render while we’re at it.

    // src/App.js
    class App extends React.Component {
        // ...
    
        render() {
            // ...
    
            return (
                <div //...>
                    <svg //...>
                        <CountyMap //... />
    
                        // markua-start-insert
                        <rect x="500" y="0"
                              width="600"
                              height="500"
                              style={{fill: 'white'}} />
                        // markua-end-insert
    
                        <Histogram //... />
                        <MedianLine //.. />
                    </svg>
    
                    // markua-start-insert
                    <Controls data={techSalaries}
                              updateDataFilter={this.updateDataFilter} />
                    // markua-end-insert
                </div>
            )
        }
    }

Rectangle, `500` to the right, `0` from top, `600` wide and `500` tall,
with a white background. Gives the histogram an opaque background, so it
doesn’t matter what the map is doing.

We render the `Controls` component just after `</svg>` because it’s not
an SVG component – it uses normal HTML. Unlike other components, it
needs our entire dataset as `data`. We use the `updateDataFilter` prop
to say which callback function it should call when a new filter is
ready.

If this seems roundabout … I’ve seen worse. The callbacks approach makes
our app easier to componentize and keeps the code relatively unmessy.
Imagine putting everything we’ve done so far in `App`\! :laughing:
