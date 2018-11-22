
{\#tie-datasets-together}

## Step 4: Tie the datasets together

If you add a `console.log` to the `.then` callback above, you’ll see a
bunch of data. Each argument - `us`, `countyNames`, `medianIncomes`,
`techSalaries`, `USstateNames` - holds a parsed dataset from the
corresponding file.

To tie them together and prepare a dictionary for `setState` back in the
`App` component, we need to add some logic. We’re building a dictionary
of county household incomes and removing any empty salaries.

    // src/DataHandling.js
    ]).then(([us, countyNames, medianIncomes, techSalaries, USstateNames]) => {
        let medianIncomesMap = {};
    
        medianIncomes.filter(d => _.find(countyNames,
                                         {name: d['countyName']}))
                     .forEach((d) => {
                         d['countyID'] = _.find(countyNames,
                                                {name: d['countyName']}).id;
                         medianIncomesMap[d.countyID] = d;
                     });
    
        techSalaries = techSalaries.filter(d => !_.isNull(d));
    
        callback({
            usTopoJson: us,
            countyNames: countyNames,
            medianIncomes: medianIncomesMap,
            medianIncomesByCounty: _.groupBy(medianIncomes, 'countyName'),
            medianIncomesByUSState: _.groupBy(medianIncomes, 'USstate'),
            techSalaries: techSalaries,
            USstateNames: USstateNames
        });
    });

Building the income map looks weird because of indentation, but it’s not
that bad. We `filter` the `medianIncomes` array to discard any incomes
whose `countyName` we can’t find. I made sure they were all unique when
I built the datasets.

We walk through the filtered array with a `forEach`, find the correct
`countyID`, and add each entry to `medianIncomesMap`. When we’re done,
we have a large dictionary that maps county ids to their household
income data.

Then we filter `techSalaries` to remove any empty values where the
`cleanSalaries` function returned `null`. That happens when a salary is
either undefined or absurdly high.

When our data is ready, we call our `callback` with a dictionary of the
new datasets. To make future access quicker, we use `_.groupBy` to build
dictionary maps of counties by county name and by US state.

You should now see how many salary entries the shortened dataset
contains.

![Data loaded
screenshot](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/data-loaded-screenshot.png)

If that didn’t work, try comparing your changes to this [diff on
Github](https://github.com/Swizec/react-d3js-step-by-step/commit/9f113cdd3bc18535680cb5a4e87a3fd45743c9ae).
