
## Step 5: Add US state and Job Title filters

With all that done, we can add two more filters: US states and job
titles. Our `App` component is already set up to use them, so we just
have to add them to the `Controls` component.

We’ll start with the `render` method, then handle the parts I said
earlier would look repetitive.

    // src/components/Controls/index.js
    class Controls extends Component {
        // ...
        render() {
            const { data } = this.props;
    
            const years = new Set(data.map(d => d.submit_date.getFullYear())),
                  // markua-start-insert
                  jobTitles = new Set(data.map(d => d.clean_job_title)),
                  USstates = new Set(data.map(d => d.USstate));
                  // markua-end-insert
    
            return (
                <div>
                    <ControlRow data={data}
                                toggleNames={Array.from(years.values())}
                                picked={this.state.year}
                                updateDataFilter={this.updateYearFilter}
                                />
    
                // markua-start-insert
                    <ControlRow data={data}
                                toggleNames={Array.from(jobTitles.values())}
                                picked={this.state.jobTitle}
                                updateDataFilter={this.updateJobTitleFilter} />
    
                    <ControlRow data={data}
                                toggleNames={Array.from(USstates.values())}
                                picked={this.state.USstate}
                                updateDataFilter={this.updateUSstateFilter}
                                capitalize="true" />
                    // markua-end-insert
                </div>
            )
        }
    }

Ok, this part is plenty repetitive, too.

We created new sets for `jobTitles` and `USstates`, then rendered two
more `ControlRow` elements with appropriate attributes. They get
`toggleNames` for building the buttons, `picked` to know which is
active, an `updateDataFilter` callback, and we tell US states to render
capitalized.

The implementations of those `updateDataFilter` callbacks follow the
same pattern as `updateYearFilter`.

    // src/components/Controls/index.js
    
    class Controls extends React.Component {
        state = {
            yearFilter: () => true,
            jobTitleFilter: () => true,
            USstateFilter: () => true,
            year: "*",
            USstate: "*",
            jobTitle: "*"
        };
    
        updateJobTitleFilter = (title, reset) => {
            let filter = d => d.clean_job_title === title;
    
            if (reset || !title) {
                filter = () => true;
                title = "*";
            }
    
            this.setState(
                {
                    jobTitleFilter: filter,
                    jobTitle: title
                },
                () => this.reportUpdateUpTheChain()
            );
        };
    
        updateUSstateFilter = (USstate, reset) => {
            let filter = d => d.USstate === USstate;
    
            if (reset || !USstate) {
                filter = () => true;
                USstate = "*";
            }
    
            this.setState(
                {
                    USstateFilter: filter,
                    USstate: USstate
                },
                () => this.reportUpdateUpTheChain()
            );
        };
        
        // ..
    }
    
    export default Controls;

Yes, they’re basically the same as `updateYearFilter`. The only
difference is a changed `filter` function and using different keys in
`setState()`.

Why separate functions then? No need to get fancy. It would’ve made the
code harder to read.

Our last step is to add these new keys to the `reportUpdateUpTheChain`
function.

    // src/components/Controls/index.js
    
    class Controls extends React.Component {
        reportUpdateUpTheChain() {
            this.props.updateDataFilter(
                (filters => {
                    return d =>
                        filters.yearFilter(d) &&
                        filters.jobTitleFilter(d) &&
                        filters.USstateFilter(d);
                })(this.state),
                {
                    USstate: this.state.USstate,
                    year: this.state.year,
                    jobTitle: this.state.jobTitle
                }
            );
        }

We add them to the filter condition with `&&` and expand the
`filteredBy` argument.

Two more rows of filters show up.

![All the
filters](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/all-filters.png)

:clap:

Again, if it didn’t work, consult [the diff on
GitHub](https://github.com/Swizec/react-d3js-step-by-step/commit/a45c33e172297ca1bbcfdc76733eae75779ebd7f).
