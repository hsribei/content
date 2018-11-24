
## Step 1: Prep App.js

You might guess the pattern already: add an import, add a helper method
or two, update `render`.

``` javascript
// src/App.js
import Preloader from './components/Preloader';
import { loadAllData } from './DataHandling';

// markua-start-insert
import CountyMap from './components/CountyMap';
// markua-end-insert
```

That imports the `CountyMap` component from `components/CountyMap/`.
Your browser will show an error overlay about some file or another until
we’re done.

In the `App` class itself, we add a `countyValue` method. It takes a
county entry and a map of tech salaries, and it returns the delta
between median household income and a single tech salary.

``` javascript
    // src/App.js
    countyValue(county, techSalariesMap) {
        const medianHousehold = this.state.medianIncomes[county.id],
              salaries = techSalariesMap[county.name];

        if (!medianHousehold || !salaries) {
            return null;
        }

        const median = d3.median(salaries, d => d.base_salary);

        return {
            countyID: county.id,
            value: median - medianHousehold.medianIncome
        };
    }
```

We use `this.state.medianIncomes` to get the median household salary and
the `techSalariesMap` input to get salaries for a specific census area.
Then we use `d3.median` to calculate the median value for salaries and
return a two-element dictionary with the result.

`countyID` specifies the county and `value` is the delta that our
choropleth displays.

In the `render` method, we’ll:

  - prep a list of county values
  - remove the “data loaded” indicator
  - render the map

<!-- end list -->

``` javascript
// src/App.js
render() {
    // markua-start-insert
    const { countyNames, usTopoJson, techSalaries, } = this.state;
    // markua-end-insert
        
    if (techSalaries.length < 1) {
        return (
            <Preloader />
        );
    }

    // markua-start-insert
    const filteredSalaries = techSalaries,
          filteredSalariesMap = _.groupBy(filteredSalaries, 'countyID'),
          countyValues = countyNames.map(
              county => this.countyValue(county, filteredSalariesMap)
          ).filter(d => !_.isNull(d));

    let zoom = null;
    // markua-end-insert

        return (
          <div className="App container">
            // markua-start-delete
            <h1>Loaded {techSalaries.length} salaries</h1>
            // markua-end-delete
            // markua-start-insert
            <svg width="1100" height="500">
                <CountyMap usTopoJson={usTopoJson}
                           USstateNames={USstateNames}
                           values={countyValues}
                           x={0}
                           y={0}
                           width={500}
                           height={500}
                           zoom={zoom} />
            </svg>
            // markua-end-insert
          </div>
        );
}
```

We call our dataset `filteredTechSalaries` because we’re going to add
filtering in the [subchapter about adding user
controls](#user-controls). Using the proper name now means less code to
change later. The magic of foresight :smile:

We use `_.groupBy` to build a dictionary mapping each `countyID` to an
array of salaries, and we use our `countyValue` method to build an array
of counties for our map.

We set `zoom` to `null` for now. We’ll use this later.

In the `return` statement, we remove our “data loaded” indicator, and
add an `<svg>` element that’s `1100` pixels wide and `500` pixels high.
Inside, we place the `CountyMap` component with a bunch of properties.
Some dataset stuff, some sizing and positioning stuff.
