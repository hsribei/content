
# Show a Preloader

![Preloader
screenshot](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/preloader-screenshot.png)

Our preloader is a screenshot of the final result. Usually you’d have to
wait until the end of the project to make that, but I’ll just give you
mine. Starting with the preloader makes sense for two reasons:

1.  It’s nicer than looking at a blank screen while data loads
2.  It’s a good sanity check for our environment

We’re using a screenshot of the final result because the full dataset
takes a few seconds to load, parse, and render. It looks better if
visitors see something informative while they wait.

React Suspense is about to make building preloaders a whole lot better.
Adapting to the user’s network speed, built-in preload functionality,
stuff like that. More on that in the chapter on React Suspense and Time
Slicing.

Make sure you’ve installed [all dependencies](#install-dependencies) and
that `npm start` is running.

We’re building our preloader in 4 steps:

1.  Get the image
2.  Make the `Preloader` component
3.  Update `App`
4.  Load Bootstrap styles in `index.js`
