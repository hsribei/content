
# Render a Histogram of salaries

Knowing median salaries is great and all, but it doesn’t tell you much
about what you can expect. You need to know the distribution to see if
it’s more likely you’ll get 140k or 70k.

That’s what histograms are for. Give them a bunch of data, and they show
its distribution. We’re going to build one like this:

![Basic
histogram](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/basic-histogram.png)

In the shortened dataset, 35% of tech salaries fall between $60k and
$80k, 26% between $80k and $100k etc. Throwing a weighed dice with this
[random
distribution](https://en.wikipedia.org/wiki/Probability_distribution),
you’re far more likely to get 60k-80k than 120k-140k. It’s a great way
to gauge situations.

It’s where statistics like “More people die from vending machines than
shark attacks” come from. Which are you afraid of, vending machines or
sharks? Stats say your answer should be [heart
disease](https://www.cdc.gov/nchs/fastats/deaths.htm). ;)

We’ll start our histogram with some changes in `App.js`, make a
`Histogram` component using the [full-feature
approach](#full-feature-integration), add an `Axis` using the [blackbox
HOC approach](#blackbox-hoc), and finally add some styling.
