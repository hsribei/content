
## Step 2: Prep data parsing functions

We’re putting data loading logic in a separate file from `App.js`
because it’s a bunch of functions that work together and don’t have much
to do with the `App` component itself.

We start with two imports and four data parsing functions:

  - `cleanIncome`, which parses each row of household income data
  - `dateParse`, which we use for parsing dates
  - `cleanSalary`, which parses each row of salary data
  - `cleanUSStateName`, which parses US state names

<!-- end list -->

``` javascript
// src/DataHandling.js
import * as d3 from 'd3';
import _ from 'lodash';

const cleanIncome = (d) => ({
    countyName: d['Name'],
    USstate: d['State'],
    medianIncome: Number(d['Median Household Income']),
    lowerBound: Number(d['90% CI Lower Bound']),
    upperBound: Number(d['90% CI Upper Bound'])
});

const dateParse = d3.timeParse("%m/%d/%Y");

const cleanSalary = (d) => {
    if (!d['base salary'] || Number(d['base salary']) > 300000) {
        return null;
    }

    return {employer: d.employer,
            submit_date: dateParse(d['submit date']),
            start_date: dateParse(d['start date']),
            case_status: d['case status'],
            job_title: d['job title'],
            clean_job_title: d['job title'],
            base_salary: Number(d['base salary']),
            city: d['city'],
            USstate: d['state'],
            county: d['county'],
            countyID: d['countyID']
    };
}

const cleanUSStateName = (d) => ({
    code: d.code,
    id: Number(d.id),
    name: d.name
});

const cleanCounty = d => ({
    id: Number(d.id),
    name: name
});
```

You’ll see those `d3` and `lodash` imports a lot.

Our data parsing functions all follow the same approach: Take a row of
data as `d`, return a dictionary with nicer key names, cast any numbers
or dates into appropriate formats. They all start as strings.

Doing all this parsing now, keeps the rest of our codebase clean.
Handling data is always messy. You want to contain that mess as much as
possible.
