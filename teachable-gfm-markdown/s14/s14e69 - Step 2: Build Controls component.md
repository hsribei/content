
## Step 2: Build Controls component

The `Controls` component builds our filter function and `filteredBy`
dictionary based on user choices.

`Controls` renders 3 rows of buttons and builds filtering out of the
choice made on each row. That makes `Controls` kind of repetitive, but
that’s okay.

To keep this book shorter, we’re going to build everything for a `year`
filter first. Then I’ll explain how to add `USstate` and `jobTitle`
filters on a higher level. Once you have one working, the rest follows
that same pattern.

Make a `Controls` directory in `src/components/` and let’s begin. The
main `Controls` component goes in your `index.js` file.

### Stub Controls

{format: javascript, line-numbers: false, caption: “Controls stubbed for
year filter”}

    // src/components/Controls/index.js
    import React from "react";
    
    import ControlRow from "./ControlRow";
    
    class Controls extends React.Component {
        state = {
            yearFilter: () => true,
            year: "*"
        };
    
        componentDidMount() {
    
        }
    
        updateYearFilter = (year, reset) => {
            let filter = d => d.submit_date.getFullYear() === year;
    
            if (reset || !year) {
                filter = () => true;
                year = "*";
            }
    
            this.setState(
                {
                    yearFilter: filter,
                    year: year
                },
                () => this.reportUpdateUpTheChain()
            );
        };
    
        render() {
            const { data } = this.props;
        }
    }
    
    export default Controls;

We start with some imports and a `Controls` class-based component.
Inside, we define default `state` with an always-true `yearFilter` and
an asterisk for `year`.

We also need an `updateYearFilter` function, which we’ll use to update
the filter, a `reportUpdateUpTheChain` function, and a `render` method.
We’re using `reportUpdateUpTheChain` to bubble updates to our parent
component. It’s a simpler alternative to using React Context or a state
management library.

### Filter logic

{format: javascript, line-numbers: false, caption: “year filtering logic
in Controls”}

    // src/components/Controls/index.js
    class Controls extends React.Component {
        // ...
        updateYearFilter = (year, reset) => {
            let filter = d => d.submit_date.getFullYear() === year;
    
            if (reset || !year) {
                filter = () => true;
                year = "*";
            }
    
            this.setState(
                {
                    yearFilter: filter,
                    year: year
                },
                () => this.reportUpdateUpTheChain()
            );
        };
    }

`updateYearFilter` is a callback we pass into `ControlRow`. When a user
picks a year, their action triggers this function.

When that happens, we create a new partial filter function. The `App`
component uses it inside a `.filter` call like you saw earlier. We have
to return `true` for elements we want to keep and `false` for elements
we don’t.

Comparing `submit_date.getFullYear()` with `year` achieves that.

The `reset` argument lets us reset filters back to defaults. Enables
users to unselect options.

When we have the `year` and `filter`, we update component state with
`this.setState`. This triggers a re-render and calls
`reportUpdateUpTheChain` afterwards. Great use-case for the little known
setState callback :)

`reportUpdateUpTheChain` then calls `this.props.updateDataFilter`, which
is a callback method on `App`. We defined it earlier – it needs a new
filter method and a `filteredBy` dictionary.

{language: javascript, line-numbers: false, caption:
“reportUpdateUpTheChain method”}

    // src/components/Controls/index.js
    class Controls extends React.Component {
        // ...
        reportUpdateUpTheChain() {
            window.location.hash = [
                this.state.year || "*"
            ].join("-");
    
            this.props.updateDataFilter(
                (filters => {
                    return d =>
                        filters.yearFilter(d)
                })(this.state),
                {
                    year: this.state.year
                }
            );
        }
    }

Building the filter method looks tricky because we’re composing multiple
functions. The new arrow function takes a dictionary of filters as an
argument and returns a new function that `&&`s them all. We invoke it
immediately with `this.state` as the argument.

It looks silly when there’s just one filter, but I promise it makes
sense.

### Render

Great, we have the filter logic. Let’s render those rows of controls
we’ve been talking about.

{format: javascript, line-numbers: false, caption: “Render the year
ControlRow”}

    // src/components/Controls/index.js
    class Controls extends React.Component {
        // ...
        render() {
            const { data } = this.props;
    
            const years = new Set(data.map(d => d.submit_date.getFullYear()));
    
            return (
                <div>
                    <ControlRow
                        data={data}
                        toggleNames={Array.from(years.values())}
                        picked={this.state.year}
                        updateDataFilter={this.updateYearFilter}
                    />
                </div>
            );
        }
    }

Once more, this is generalized code used for a single example: the
`year` filter.

We build a `Set` of distinct years in our dataset, then render a
`ControlRow` using props to give it our `data`, a set of `toggleNames`,
a callback to update the filter, and which entry is `picked` right now.
Also known as the controlled component pattern, it enables us to
maintain the data-flows-down, events-bubble-up architecture.

If you don’t know about `Set`s, they’re an ES6 data structure that
ensures every entry is unique. Just like a mathematical set. They’re
pretty fast.
