
### Part 0: Some setup

Because decorators aren’t in the JavaScript spec, we have to tweak how
we start our project. We can still use `create-react-app`, but there’s
an additional step.

You should start a new project like
    this:

    $ create-react-app billiards-game --scripts-version custom-react-scripts

This creates a new directory with a full setup for React. Just like
you’re used to.

The addition of `--scripts-version custom-react-scripts` employs
@kitze’s
[custom-react-scripts](https://github.com/kitze/custom-react-scripts)
project to give us more configuration options. Like the ability to
enable decorators.

We enable them in the `.env` file. Add this line:

    // billiards-game/.env
    // ...
    REACT_APP_DECORATORS=true

No installation necessary. I think `custom-react-scripts` uses the
`transform-decorators-legacy` Babel plugin behind the scenes. It’s
pre-installed, and we enable it with that `.env` change.

Before we begin, you should install some other dependencies as
    well:

    $ npm install --save konva react-konva mobx mobx-react d3-timer d3-scale d3-quadtree

This gives you Konva, MobX, and the parts of D3 that we need. You’re now
ready to build the billiards game.
