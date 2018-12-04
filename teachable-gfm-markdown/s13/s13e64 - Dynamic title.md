
## Dynamic title

We begin with the title because it shows up first.

We start with an import in `App.js` and add it to the render method. You
know the drill 😄

``` javascript
// src/App.js
import CountyMap from './components/CountyMap';
import Histogram from './components/Histogram';
// markua-start-insert
import { Title } from './components/Meta';
// markua-end-insert

class App extends Component {
    state = {
        techSalaries: [],
        countyNames: [],
        medianIncomes: [],
        // markua-start-insert
        filteredBy: {
            USstate: '*',
            year: '*',
            jobTitle: '*'
        }
        // markua-end-insert
    }

    // ...

    render() {
            const { filteredBy } = this.state;
        // ..
        return (
            <div className="App container">
                // markua-start-insert
                <Title data={filteredSalaries}
                       filteredBy={filteredBy} />
                // markua-end-insert
                // ...
            </div>
        )
    }
}
```

Ok, I lied. We did a lot more than just imports and adding to render.

We also set up the `App` component for future user-controlled data
filtering. The `filteredBy` key in `state` tells us what the user is
filtering by – 3 options: `USstate`, `year`, and `jobTitle`. We set them
to “everything” by default.

We added them now so that we can immediately write our `Title` component
in a filterable way. No need to make changes later.

As you can see, `Title` takes `data` and `filteredBy` props.

### Prep Meta component

Before we begin the `Title` component, there are a few things to take
care of. Our meta components work together for a common purpose –
showing meta data. Grouping them in a directory makes sense.

We make a `components/Meta` directory and add an `index.js`. It makes
importing easier.

``` javascript
// src/components/Meta/index.js
export { default as Title } from './Title'
export { default as Description } from './Description';
```

We have to name our `Title` and `Description` re-exports because you
can’t have two default exports.

#### Get the USStatesMap file

You need the `USStatesMap` file.

It’s a big dictionary that translates US state codes to full names. You
can [get it from
Github](https://github.com/Swizec/react-d3js-step-by-step/blob/4f94fcd1c3caeb0fc410636243ca99764e27c5e6/src/components/Meta/USStatesMap.js)
and save it as `components/Meta/USStatesMap.js`.

We’ll use it when creating titles and descriptions.

### Implement Title

We’re building two types of titles based on user selection. If both
`years` and `US state` were selected, we return `In {US state}, the
average {job title} paid ${mean}/year in {year}`. If not, we return
`{job title} paid ${mean}/year in {state} in {year}`.

I know, it’s confusing. They look like the same sentence turned around.
Notice the *and*. First option when *both* are selected, second when
either/or.

We start with imports, a stub, and a default export.

``` javascript
// src/components/Meta/Title.js
import React, { Component } from 'react';
import { scaleLinear } from 'd3-scale';
import { mean as d3mean, extent as d3extent } from 'd3-array';

import USStatesMap from './USStatesMap';

class Title extends Component {
    get yearsFragment() {
    }

    get USstateFragment() {
    }

    get jobTitleFragment() {
    }

    get format() {
    }

    render() {
    }
}

export default Title;
```

We import only what we need from D3’s `d3-scale` and `d3-array`
packages. I consider this best practice until you’re importing so much
that it gets messy to look at.

In the `Title` component, we have 4 getters and a render. Getters are
ES6 functions that work like dynamic properties. You specify a function
without arguments, and you use it without `()`. It’s neat.

#### The getters

1.  `yearsFragment` describes the selected year
2.  `USstateFragment` describes the selected US state
3.  `jobTitleFragment` describes the selected job title
4.  `format` returns a number formatter

We can implement `yearsFragment`, `USstateFragment`, and `format` in one
code sample. They’re short.

``` javascript
// src/components/Meta/Title.js
class Title extends Component {
    get yearsFragment() {
        const year = this.props.filteredBy.year;

        return year === '*' ? "" : `in ${year}`;
    }

    getteFragment() {
        const USstate = this.props.filteredBy.USstate;

        return USstate === '*' ? "" : USStatesMap[USstate.toUpperCase()];
    }

    // ...

    get format() {
        return scaleLinear()
                 .domain(d3extent(this.props.data, d => d.base_salary))
                 .tickFormat();
    }
```

In both `yearsFragment` and `USstateFragment`, we get the appropriate
value from Title’s `filteredBy` prop, then return a string with the
value or an empty string.

We rely on D3’s built-in number formatters to build `format`. Linear
scales have the one that turns `10000` into `10,000`. Tick formatters
don’t work well without a `domain`, so we define it. We don’t need a
range because we never use the scale itself.

`format` returns a function, which makes it a [higher order
function](https://en.wikipedia.org/wiki/Higher-order_function). Being a
getter makes it really nice to use: `this.format()`. Looks just like a
normal function call :D

The `jobTitleFragment` getter is conceptually no harder than
`yearsFragment` and `USstateFragment`, but it comes with a few more
conditionals.

``` javascript
// src/components/Meta/Title.js

class Title extends Component {
    // ...
    get jobTitleFragment() {
        const { jobTitle, year } = this.props.filteredBy;
        let title = "";

        if (jobTitle === "*") {
            if (year === "*") {
                title = "The average H1B in tech pays";
            } else {
                title = "The average tech H1B paid";
            }
        } else {
            title = `Software ${jobTitle}s on an H1B`;
            if (year === "*") {
                title += " make";
            } else {
                title += " made";
            }
        }

        return title;
    }
    // ...
}
```

{caption: “Title.jobTitleFragment”}

``` 

//
// Example 1
//
// src/components/Meta/Title.js
import React, { Component } from 'react';
import { scaleLinear } from 'd3-scale';
import { mean as d3mean, extent as d3extent } from 'd3-array';

import USStatesMap from './USStatesMap';

class Title extends Component {
    get yearsFragment() {
    }

    get USstateFragment() {
    }

    get jobTitleFragment() {
    }

    get format() {
    }

    render() {
    }
}

export default Title;


//
// Example 2
//
// src/components/Meta/Title.js
class Title extends Component {
    get yearsFragment() {
        const year = this.props.filteredBy.year;

        return year === '*' ? "" : `in ${year}`;
    }

    getteFragment() {
        const USstate = this.props.filteredBy.USstate;

        return USstate === '*' ? "" : USStatesMap[USstate.toUpperCase()];
    }

    // ...

    get format() {
        return scaleLinear()
                 .domain(d3extent(this.props.data, d => d.base_salary))
                 .tickFormat();
    }
}

//
// Example 3
//
// src/components/Meta/Title.js
class Title extends Component {
    // ...
    get jobTitleFragment() {
        const { jobTitle, year } = this.props.filteredBy;
        let title = "";

        if (jobTitle === '*') {
            if (year === '*') {
                title = "The average H1B in tech pays";
            }else{
                title = "The average tech H1B paid";
            }
        }else{
            if (jobTitle === '*') {
                title = "H1Bs in tech pay";
            }else{
                title = `Software ${jobTitle}s on an H1B`;

                if (year === '*') {
                    title += " make";
                }else{
                    title += " made";
                }
            }
        }

        return title;
    }
    // ...
}

//
// Example 4
//
// src/components/Meta/Title.js
class Title extends Component {
    // ...
    render() {
        const mean = this.format(d3mean(this.props.data, d => d.base_salary));

        let title;

        if (this.yearsFragment && this.USstateFragment) {
            title = (
                <h2>
                    In {this.USstateFragment}, {this.jobTitleFragment}
                    ${mean}/year {this.yearsFragment}
                </h2>
            );
        }else{
            title = (
                <h2>
                    {this.jobTitleFragment} ${mean}/year
                    {this.USstateFragment ? `in ${this.stateFragment}` : ''}
                    {this.yearsFragment}
                </h2>
            );
        }

        return title;
    }
}
```

We’re dealing with the `(jobTitle, year)` combination. Each influences
the other when building the fragment for a total 4 different options.

#### The render

We put all this together in the `render` method. A conditional decides
which of the two situations we’re in, and we return an `<h2>` tag with
the right text.

``` javascript
// src/components/Meta/Title.js
class Title extends Component {
    // ...
    render() {
        const mean = this.format(d3mean(this.props.data, d => d.base_salary));

        let title;

        if (this.yearsFragment && this.USstateFragment) {
            title = (
                <h2>
                    In {this.USstateFragment}, {this.jobTitleFragment}
                    ${mean}/year {this.yearsFragment}
                </h2>
            );
        }else{
            title = (
                <h2>
                    {this.jobTitleFragment} ${mean}/year
                    {this.USstateFragment ? `in ${this.stateFragment}` : ''}
                    {this.yearsFragment}
                </h2>
            );
        }

        return title;
    }
}
```

Calculate the mean value using `d3.mean` with a value accessor, turn it
into a pretty number with `this.format`, then use one of two string
patterns to make a `title`.

And a title appears.

![Dataviz with
title](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/dataviz-with-title.png)

If it doesn’t, consult [this diff on
Github](https://github.com/Swizec/react-d3js-step-by-step/commit/4f94fcd1c3caeb0fc410636243ca99764e27c5e6).
