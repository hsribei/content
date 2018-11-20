
## Step 4: HistogramBar (sub)component

Before our histogram shows up, we need another component:
`HistogramBar`. We *could* have shoved all of it in the `makeBar`
function, but it makes sense to keep separate. Better future
flexibility.

You can write small components like this in the same file as their main
component. They’re not reusable since they fit a specific use-case, and
they’re small enough so your files don’t get too crazy.

But in the interest of readability, let’s make a `HistogramBar` file.

``` javascript
// src/components/Histogram/HistogramBar.js
import React from "react";

const HistogramBar = ({ percent, x, y, width, height }) => {
    let translate = `translate(${x}, ${y})`,
        label = percent.toFixed(0) + "%";

    if (percent < 1) {
        label = percent.toFixed(2) + "%";
    }

    if (width < 20) {
        label = label.replace("%", "");
    }

    if (width < 10) {
        label = "";
    }

    return (
        <g transform={translate} className="bar">
            <rect
                width={width}
                height={height - 2}
                transform="translate(0, 1)"
            />
            <text textAnchor="end" x={width - 5} y={height / 2 + 3}>
                {label}
            </text>
        </g>
    );
};

export default HistogramBar;
```

Pretty long for a functional component. Most of it goes into deciding
how much precision to render in the label, so it’s okay.

We start with an SVG translate and a default `label`. Then we update the
label based on the bar size and its value.

When we have a label we like, we return a `<g>` grouping element with a
rectangle and a text. Both positioned based on the `width` and `height`
of the bar.

Make sure to import `HistogramBar` in the main `Histogram` file.

``` javascript
// src/components/Histogram/Histogram.js

import HistogramBar from './HistogramBar'
```

You should now see a histogram.

![Histogram without
axis](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/histogram-without-axis.png)
