
# Build scalable dataviz components with full integration

As useful as blackbox components are, we need something better if we
want to leverage React’s rendering engine. The blackbox approach in
particular struggles with scale. The more charts and graphs and
visualizations on your screen, the slower it gets.

Someone once came to my workshop and said *“We used the blackbox
approach and it takes several seconds to re-render our dashboard on any
change. I’m here to learn how to do it better.”*

In our full-feature integration, React does the rendering and D3
calculates the props.

Our goal is to build controlled components that listen to their props
and reconcile that with D3’s desire to use a lot of internal state.

There are two situations we can find ourselves in:

1.  We know for a fact our component’s props never change
2.  We think props could change

It’s easiest to show you with an example.

Let’s build a scatterplot step by step. Take a random array of
two-dimensional data, render in a loop. Make magic.

Something like this 👇

![A simple
scatterplot](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/scatterplot.png)

You’ve already built the axes\! Copy pasta time.
