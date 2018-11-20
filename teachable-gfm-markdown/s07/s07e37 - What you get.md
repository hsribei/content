
## What you get

Running `create-react-app` installs tools and libraries. There’s around
80MB of them as of October 2016. This is why using a generator is easier
than slogging through on your own.

Crucially, there is a single dependency in your project –
`react-scripts`. But it gives you everything you need to build modern
React apps.

  - **Webpack** - a module bundler and file loader. It turns your app
    into a single JavaScript file, and it even lets you import images
    and styles like they were code.
  - **Babel** - a JavaScript transpiler. It turns your modern JS code
    (ES6, ECMAScript2015, 2016, ES7, whatever you call it) into code
    that can run on real world browsers. It’s the ecosystem’s answer to
    slow browser adoption.
  - **ESLint** - linting\! It annoys you when you write code that is
    bad. This is a good thing. :smile:
  - **Jest** - a test runner. Having tests set up from the beginning of
    a project is a good idea. We won’t really write tests in this book,
    but I will show you how it’s done.

All tools come preconfigured with sane defaults and ready to use. You
have no idea how revolutionary this is. Check the appendixes to see how
hard setting up an environment used to be. I’m so happy that
`create-react-app` exists.

-----

Besides the toolchain, `create-react-app` also gives you a default
directory structure and a ton of helpful docs.

You should read the `README.md` file. It’s full of great advice, and it
goes into more detail about the app generator than we can get into here.
The team also makes sure it’s kept up-to-date.

All of the code samples in this book are going to follow the default
directory structure. Public assets like HTML files, datasets, and some
images go into `public/`. Code goes into `src/`.

Code is anything that we `import` or `require()`. That includes
JavaScript files themselves, as well as stylesheets and images.

You can poke around `src/App.js` to see how it’s structured and what
happens when you change something. You’re going to change bits and
pieces of this file as you go through the book.

I suggest you keep `npm start` running at all times while coding. The
browser will refresh on every code change, so you’ll be able to see
instant results.
