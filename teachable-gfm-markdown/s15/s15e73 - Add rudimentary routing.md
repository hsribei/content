
# Rudimentary routing

Imagine this. A user finds your dataviz, clicks around, and finds an
interesting insight. They share it with their friends.

*“See\! I was right\! This link proves it.”*

*“Wtf are you talking about?”*

*“Oh… uuuuh… you have to click this and then that and then you’ll see.
I’m legit winning our argument.”*

*“Wow\! Kim Kardashian just posted a new snap with her dog.”*

Let’s help our intrepid user out and make our dataviz linkable. We
should store the current `filteredBy` state in the URL and be able to
restore from a link.

There are many ways to achieve this.
[ReactRouter](https://github.com/ReactTraining/react-router) comes to
mind, but the quickest is to implement our own rudimentary routing.
We’ll add some logic to manipulate and read the URL hash.

The easiest place to put this logic is next to the existing filter logic
inside the `Controls` component. Better places exist from a “low-down
components shouldn’t play with global state” perspective, but that’s
okay.

``` javascript
// src/components/Controls/index.js

class Controls extends React.Component {
    // ..

    componentDidMount() {
        let [year, USstate, jobTitle] = window.location.hash
            .replace("#", "")
            .split("-");

        if (year !== "*" && year) {
            this.updateYearFilter(Number(year));
        }
        if (USstate !== "*" && USstate) {
            this.updateUSstateFilter(USstate);
        }
        if (jobTitle !== "*" && jobTitle) {
            this.updateJobTitleFilter(jobTitle);
        }
    }
    
    // ..
    reportUpdateUpTheChain() {
        window.location.hash = [
            this.state.year || "*",
            this.state.USstate || "*",
            this.state.jobTitle || "*"
        ].join("-");

        // ..
    }
```

We use the `componentDidMount` lifecycle hook to read the URL when
`Controls` first render on our page. Presumably when the page loads, but
it could be later. It doesn’t really matter *when*, just that we update
our filter the first chance we get.

`window.location.hash` gives us the hash part of the URL. We clean it up
and split it into three parts: `year`, `USstate`, and `jobTitle`. If the
URL is `localhost:3000/#2013-CA-manager`, then `year` becomes `2013`,
`USstate` becomes `CA`, and `jobTitle` becomes `manager`.

We make sure each value is valid and use our existing filter update
callbacks to update the visualization. Just as if it was the user
clicking a button.

In `reportUpdateUpTheChain`, we make sure to update the URL hash.
Assigning a new value to `window.location.hash` takes care of that.

You should see the URL changing as you click around.

![Changing URL
hash](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/changing-url.png)

There’s a bug with some combinations in 2013 that don’t have enough
data. It will go away when we use the full dataset.

If it doesn’t work at all, consult the [diff on
Github](https://github.com/Swizec/react-d3js-step-by-step/commit/2e8fb070cbee5f1e942be8ea42fa87c6c0379a9b).
