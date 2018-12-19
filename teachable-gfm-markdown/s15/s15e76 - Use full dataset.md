
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
        // Delete the line(s) between here...
        .defer(d3.csv, 'data/h1bs-2012-2016-shortened.csv', cleanSalary)
        // ...and here.
        // Insert the line(s) between here...
        .defer(d3.csv, 'data/h1bs-2012-2016.csv', cleanSalary)
        // ...and here.
```

We change the file name, and that’s that. Full dataset locked and
loaded. Dataviz ready to go.
