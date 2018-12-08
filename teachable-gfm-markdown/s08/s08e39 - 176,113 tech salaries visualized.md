
# A big example project - 176,113 tech salaries visualized

We’re going to build
this:

![](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/full-dataviz.png)

An interactive visualization dashboard app with a choropleth map and a
histogram comparing tech salaries to median household income in the
area. Users can filter by year, job title, or US state to get a better
view.

![](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/interaction-dataviz.png)

It’s going to be great.

At this point, I assume you’ve used `create-react-app` to set up your
environment. Check the [getting
started](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6888622#getting-started)
section if you haven’t. I’m also assuming you’ve read the [basics
chapter](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6887708#the-meat-start).
I’m still going to explain what we’re doing, but knowing the basics
helps.

I suggest you follow along, keep `npm start` running, and watch your
visualization change in real time as you code. It’s rewarding as hell.

If you get stuck, you can use my [react-d3js-step-by-step Github
repo](https://github.com/Swizec/react-d3js-step-by-step) to jump between
steps. The [9
tags](https://github.com/Swizec/react-d3js-step-by-step/releases)
correspond to the code at the end of each step. Download the first tag
and run `npm install` to skip the initial setup.

If you want to see how this project evolved over 22 months, check [the
original repo](https://github.com/Swizec/h1b-software-salaries). The
[modern-code](https://github.com/Swizec/h1b-software-salaries/tree/modern-code)
branch has the code you’re about to build.
