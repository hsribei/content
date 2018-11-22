
# Render a choropleth map of the US

With our data in hand, it’s time to draw some pictures. A choropleth map
will show us the best places to be in tech.

We’re showing the delta between median household salary in a statistical
county and the average salary of an individual tech worker on a visa.
The darker the blue, the higher the difference.

The more a single salary can out-earn an entire household, the better
off you are.

![Choropleth map with shortened
dataset](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/choropleth-map-shortened-dataset.png)

There’s a lot of gray on this map because the shortened dataset doesn’t
have that many counties. Full dataset is going to look better, I
promise.

Turns out immigration visa opportunities for techies aren’t evenly
distributed throughout the country. Who knew?

Just like before, we’re going to start with changes in our `App`
component, then build the new bit. This time, a `CountyMap` component
spread over three files:

  - `CountyMap/index.js`, to make imports easier
  - `CountyMap/CountyMap.js`, for overall map logic
  - `CountyMap/County.js`, for individual county polygons
