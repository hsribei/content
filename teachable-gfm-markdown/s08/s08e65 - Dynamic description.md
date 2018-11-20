
## Dynamic description

You know what? The dynamic description component is pretty much the same
as the title. It’s just longer and more complex and uses more code. It’s
interesting, but not super relevant to the topic of this book.

So rather than explain it all here, I’m going to give you a link to the
[diff on
Github](https://github.com/Swizec/react-d3js-step-by-step/commit/032fe6e988b903b6d86a60d2f0404456785e180f)

We use the same approach as before:

1.  Add imports in `App.js`
2.  Add component to `App` render
3.  Add re-export to `components/Meta/index.js`
4.  Implement component in `components/Meta/Description.js`
5.  Use getters for sentence fragments
6.  Play with conditionals to construct different sentences

142 lines of mundane code.

All the interesting complexity goes into finding the richest city and
county. That part looks like this:

``` javascript
// src/components/Meta/Description.js
get countyFragment() {
    const byCounty = _.groupBy(this.props.data, 'countyID'),
          medians = this.props.medianIncomesByCounty;

    let ordered = _.sortBy(
        _.keys(byCounty)
         .map(county => byCounty[county])
         .filter(d => d.length/this.props.data.length > 0.01),
        items => d3mean(items,
                        d => d.base_salary) - medians[items[0].countyID][0].medianIncome);
    
    let best = ordered[ordered.length-1],
        countyMedian = medians[best[0].countyID][0].medianIncome;

    // ...
}
```

We group the dataset by county, then sort counties by their income
delta. We look only at counties that are bigger than 1% of the entire
dataset. And we define income delta as the difference between a county’s
median household income and the median tech salary in our dataset.

This code is not super efficient, but it gets the job done. We could
optimize by just looking for the max value, for example.

Similar code handles finding the best city.

### render the description

I recommend copying the [`Description` component from
GitHub](https://github.com/Swizec/react-d3js-step-by-step/commit/032fe6e988b903b6d86a60d2f0404456785e180f).
Most of it has little to do with React and data visualization. It’s all
about combining sentence fragments based on props.

You then render the Description like this:

``` javascript
// src/components/App.js

import { Title, Description } from "./components/Meta";

// ..

<Description
    data={filteredSalaries}
    allData={techSalaries}
    filteredBy={filteredBy}
    medianIncomesByCounty={this.state.medianIncomesByCounty}
/>
```

![Dataviz with Title and
Description](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/dataviz-with-description.png)

Another similar component is the `GraphDescription`. It shows a small
description on top of each chart that explains how to read the picture.
Less “Here’s a key takeaway”, more “color means X”.

You can follow this [diff on
Github](https://github.com/Swizec/react-d3js-step-by-step/commit/37b5222546c3f8f58f3147ce0bef6a3c1afe1b47)
to implement it. Same approach as `Title` and `Description`.

![Dataviz with all
descriptions](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/dataviz-with-all-descriptions.png)
