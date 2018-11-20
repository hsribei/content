
## Step 3: Load the datasets

Now we can use D3 to load our data with fetch requests.

    // src/DataHandling.js
    export const loadAllData = (callback = _.noop) => {
        Promise.all([
            d3.json("data/us.json"),
            d3.csv("data/us-county-names-normalized.csv", cleanCounty),
            d3.csv("data/county-median-incomes.csv", cleanIncome),
            d3.csv("data/h1bs-2012-2016-shortened.csv", cleanSalary),
            d3.tsv("data/us-state-names.tsv", cleanUSStateName)
        ]).then(([us, countyNames, medianIncomes, techSalaries, USstateNames]) => {
    
        });
    };

Here you can see another ES6 trick: default argument values. If
`callback` is undefined, we set it to `_.noop` - a function that does
nothing. This lets us later call `callback()` without worrying whether
it’s defined.

In version 5, D3 updated its data loading methods to use promises
instead of callbacks. You can load a single file using
`d3.csv("filename").then(data => ....`. The promise resolves with your
data, or throws an error.

If you’re on D3v4 and can’t upgrade, you’ll have to import from
`d3-fetch`.

Each `d3.csv` call makes a fetch request, parses the fetched CSV file
into an array of JavaScript dictionaries, and passes each row through
the provided cleanup function. We pass all median incomes through
`cleanIncome`, salaries through `cleanSalary`, etc.

To load multiple files, we use `Promise.all` with a list of unresolved
promises. Once resolved, our `.then` handler gets a list of results. We
use array destructuring to expand that list into our respective datasets
before running some more logic to tie them together.

D3 supports formats like `json`, `csv`, `tsv`, `text`, and `xml` out of
the box. You can make it work with custom data sources through the
underlying `request` API.

PS: we’re using the shortened salary dataset to make page reloads faster
while building our project.
