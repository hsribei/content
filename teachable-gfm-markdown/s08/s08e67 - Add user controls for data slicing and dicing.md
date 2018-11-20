
# Add user controls for data slicing and dicing

Now comes the fun part. All that extra effort we put into making our
components aware of filtering, and it all comes down to this: User
controls.

Here’s what we’re building:

![User controlled
filters](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/controls.png)

It’s a set of filters for users to slice and dice our visualization. The
shortened dataset gives you 2 years, 12 job titles, and 50 US states.
You’ll get 5+ years and many more job titles with the full dataset.

We’re using the [architecture we discussed](#basic-architecture) earlier
to make it work. Clicking buttons updates a filter function and
communicates it all the way up to the `App` component. `App` then uses
it to update `this.state.filteredSalaries`, which triggers a re-render
and updates our dataviz.

![Architecture
sketch](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/architecture_callbacks.jpg)

We’re building controls in 4 steps, top to bottom:

1.  Update `App.js` with filtering and a `<Controls>` render
2.  Build a `Controls` component, which builds the filter based on
    inputs
3.  Build a `ControlRow` component, which handles a row of buttons
4.  Build a `Toggle` component, which is a button

We’ll go through the files linearly. That makes them easier for me to
explain and easier for you to understand, but that also means there’s
going to be a long period where all you’re seeing is an error like this:

![Controls error during
coding](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/controls-error.png)

If you want to see what’s up during this process, remove an import or
two and maybe a thing from render. For instance, it’s complaining about
`ControlRow` in this screenshot. Remove the `ControlRow` import on top
and delete `<ControlRow ... />` from render. The error goes away, and
you see what you’re doing.
