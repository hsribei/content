
## Make sure you have node.js

Modern JavaScript development runs on node.js. Your code still ends up
in a browser, but there are a few steps it has to go through before it
gets there. Those toolchains run on node.js.

Go to [nodejs.org](https://nodejs.org/en/), download one of the latest
versions, and run the installer. You can pick the more stable LTS
(long-term-support) version, or the more gimme-all-the-features version.
JavaScript toolchains run fine in both.

Server-side rendering might be easier with the fancy-pants version. More
on that later.

## Install create-react-app

Great, you have node and you’re ready to go.

Run this command in a terminal:

    $ npm install -g create-react-app

This installs a global npm package called `create-react-app`. Its job is
to create a directory and install `react-scripts`.

Confusing, I know.

You can think of `create-react-app` and `react-scripts` as two parts of
the same construction. The first is a lightweight script that you can
install globally and never update; the second is where the work happens.
You want this one to be fresh every time you start a new app.

Keeping a global dependency up to date would suck, especially
considering there have been six major updates since Facebook first
launched `create-react-app` in July 2016.

## Run create-react-app

Superb\! You have `create-react-app`. Time to create an app and get
started with some coding.

Run this in a terminal:

    $ create-react-app react-d3js-example

Congratulations\! You just created a React app. *With* all the setup and
the fuss that goes into using a modern JavaScript toolchain.

Your next step is to run your app:

    $ cd react-d3js-example
    $ npm start

A browser tab should open with a page that looks like this:

![Initial React
app](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/initial-app.png)

If that didn’t work, then something must have gone terribly wrong. You
should consult [the official
docs](https://github.com/facebookincubator/create-react-app). Maybe that
will help.
