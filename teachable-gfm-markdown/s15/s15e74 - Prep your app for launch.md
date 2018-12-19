
# Prep for launch

You’ve built a great visualization. Congratz\! Time to put it online and
share with the world.

To do that, we’re going to use Github Pages because our app is a
glorified static website. There’s no backend, so all we need is
something to serve our HTML, JavaScript, and CSV. Github Pages is
perfect for that.

It’s free, works well with `create-react-app`, and can withstand a lot
of traffic. You don’t want to worry about traffic when your app gets to
the top of HackerNews or Reddit.

There are a few things we should take care of:

  - setting up deployment
  - adding a page title
  - adding some copy
  - Twitter and Facebook cards
  - an SEO tweak for search engines
  - use the full dataset

## Setting up deployment

You’ll need a Github repository. If you’re like me, writing all this
code without version control or off-site backup made you nervous, so you
already have one.

For everyone else, head over to Github, click the green `New Repository`
button and give it a name. Then copy the commands it gives you and run
them in your console.

It should be something like this:

    $ git init
    $ git commit -m "My entire dataviz"
    $ git remote add origin git://github ...
    $ git push origin -u master

If you’ve been git-ing locally without pushing, then you only need the
`git remote add` and `git push origin` commands. This puts your code on
Github. Great idea for anything you don’t want to lose if your computer
craps out.

Every Github repository comes with an optional Github Pages setup. The
easiest way for us to use it is with the `gh-pages` npm module.

Install it with this command:

    $ npm install --save-dev gh-pages

Add two lines to package.json:

``` json
// package.json
// Insert the line(s) between here...
"homepage": "https://<your username>.github.io/<your repo name>"
// ...and here.
"scripts": {
    "eject": "react-scripts eject",
    // Insert the line(s) between here...
    "deploy": "npm run build && gh-pages -d build"
    // ...and here.
}
```

We’ve been ignoring the `package.json` file so far, but it’s a pretty
important file. It specifies all of our project’s dependencies, meta
data for npm, and scripts that we run locally. This is where `npm start`
is defined, for instance.

We add a `deploy` script that runs `build` and a `gh-pages` deploy, and
we specify a `homepage` URL. The URL tells our build script how to set
up URLs for static files in `index.html`.

Github Pages URLs follow a `https://<your username>.github.io/<your repo
name>` schema. For instance, mine is
`https://swizec.github.io/react-d3js-step-by-step`. Yours will be
different.

You can deploy with `npm run deploy`. Make sure all changes are
committed. We’ll do it together when we’re done setting up.
