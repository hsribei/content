
{\#histogram-css}

## Step 2: CSS changes

As mentioned, opinions vary on the best approach to styling React apps.
Some say stylesheets per component, some say styling inside JavaScript,
others swear by global app styling.

The truth is somewhere in between. Do what fits your project and your
team. We’re using global stylesheets because it’s the simplest.

Create a new file `src/style.css` and add these 29 lines:

{format: css, line-numbers: false, caption: “style.css stylesheet”}

    .histogram .bar rect {
        fill: steelblue;
        shape-rendering: crispEdges;
    }
    
    .histogram .bar text {
        fill: #fff;
        font: 12px sans-serif;
    }
    
    button {
        margin-right: .5em;
        margin-bottom: .3em !important;
    }
    
    .row {
        margin-top: 1em;
    }
    
    .mean text {
        font: 11px sans-serif;
        fill: grey;
    }
    
    .mean path {
        stroke-dasharray: 3;
        stroke: grey;
        stroke-width: 1px;
    }

We won’t go into details about the CSS here. Many better books have been
written about it.

In broad strokes:

  - we’re making `.histogram` rectangles – the bars – blue
  - labels white `12px` font
  - `button`s and `.row`s have some spacing
  - the `.mean` line is a dotted grey with gray `11px` text.

More CSS than we need for just the histogram, but we’re already here so
might as well write it now.

Adding our CSS before building the Histogram means it’s going to look
beautiful the first time around.
