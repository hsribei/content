
## Full dataset

One more step left. Use the whole dataset\!

Go into `src/DataHandling.js` and change one line:

``` javascript
// Example 4
//
// src/DataHandling.js
export const loadAllData = (callback = _.noop) => {
    d3.queue()
        // ..
        // markua-start-delete
        .defer(d3.csv, 'data/h1bs-2012-2016-shortened.csv', cleanSalary)
        // markua-end-delete
        // markua-start-insert
        .defer(d3.csv, 'data/h1bs-2012-2016.csv', cleanSalary)
        // markua-end-insert
```

We change the file name, and that’s that. Full dataset locked and
loaded. Dataviz ready to go.
