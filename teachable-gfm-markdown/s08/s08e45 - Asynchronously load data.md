
# Asynchronously load data

Great\! We have a preloader. Time to load some data.

We’ll use D3’s built-in data loading methods and tie their promises into
React’s component lifecycle. You could talk to a REST API in the same
way. Neither D3 nor React care what the datasource is.

First, you need some data.

Our dataset comes from a few sources. Tech salaries are from
[h1bdata.info](http://h1bdata.info), median household incomes come from
the US census data, and I got US geo map info from Mike Bostock’s github
repositories. Some elbow grease and python scripts tied them all
together.

You can read about the scraping on my blog
[here](https://swizec.com/blog/place-names-county-names-geonames/swizec/7083),
[here](https://swizec.com/blog/facts-us-household-income/swizec/7075),
and
[here](https://swizec.com/blog/livecoding-24-choropleth-react-js/swizec/7078).
But it’s not the subject of this book.
