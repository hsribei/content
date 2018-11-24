
## Step 4: Build Toggle component

Last piece of the puzzle – the Toggle component. A button that turns on
and off.

``` javascript
// src/components/Controls/Toggle.js
import React from "react";

const Toggle = ({ label, name, value, onClick }) => {
    let className = "btn btn-default";

    if (value) {
        className += " btn-primary";
    }

    return (
        <button className={className} onClick={() => onClick(name, !value)}>
            {label}
        </button>
    );
};

export default Toggle;
```

Import React to enable JSX and make a functional `Toggle` component.
It’s fully controlled and takes event handlers as callbacks in props.
No need for a class.

We set up a Bootstrap classname: `btn` and `btn-default` make an element
look like a button, the conditional `btn-primary` makes it blue. If
you’re not familiar with Bootstrap classes, you should check [their
documentation](http://getbootstrap.com/).

Then we render a `<button>` tag and, well, that’s it. A row of year
filters appears in the browser. `onClick` passes

![A row of year
filters](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/year-filter-row.png)

Click on a button and the `onClick` handler passes a toggle’d value to
its parent via the `onClick` callback. This triggers an update in
`Controls`, which calls `reportUpdateUpTheChain`, which in turn updates
state in `App` and re-renders our button with the new value toggling its
color on or off.

If that didn’t work, consult this [diff on
GitHub](https://github.com/Swizec/react-d3js-step-by-step/commit/a45c33e172297ca1bbcfdc76733eae75779ebd7f).
