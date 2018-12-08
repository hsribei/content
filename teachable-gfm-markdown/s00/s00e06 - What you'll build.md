
# Visualizing data with React and d3.js

Welcome to the main part of React + D3 2018. We’re going to talk a
little theory, learn some principles, then get our hands dirty with some
examples. Through this book you’re going to build:

  - [A few small components in
    Codesandbox](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6887721#basic-approach)
  - [A choropleth
    map](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6888908#choropleth-map)
  - [An interactive
    histogram](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6888919#histogram-of-salaries)
  - [A bouncing
    ball](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6906619#a-bouncing-ball-game-loop-example)
  - [A swipe
    transition](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6906645#swipe-transition)
  - [An animated
    alphabet](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6906659#animated-alphabet)
  - [A simple particle generator with
    Redux](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6906668#animating-react-redux)
  - [A particle generator pushed to 20,000 elements with
    canvas](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6906689#canvas-react-redux)
  - [Billiards simulation with MobX and
    canvas](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6906696#build-a-declarative-billiards-simulation-with-mobx-canvas-and-konva)
  - [A dancing fractal
    tree](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/8456113#fractal-tree)

Looks random, right? Bear with me. Examples build on each other.

The first few examples teach you about static data visualizations and
the basics of merging React and D3 using two different approaches.

The choropleth and histogram visualizations teach you about
interactivity and components working together.

You learn about game loop and transition animations through two types of
bouncing balls. Followed by more complex enter/exit transitions with an
animated alphabet.

We look at how far we can push React’s performance with a simple
particle generator and a dancing fractal tree. A billiards game helps us
learn about complex canvas manipulation.

Throughout these examples, we’re going to use **React 16**, compatible
with **React 17**, **D3v5**, and modern **ES6+** JavaScript syntax. The
particle generator also uses **Redux** for the game loop and the
billiards simulation uses MobX. We use **Konva** for complex canvas
stuff.

That way you can build experience in large portions of the React
ecosystem so you can choose what you like best.

Don’t worry, if you’re not comfortable with modern JavaScript syntax
just yet. By the end of this book, you’re gonna love it\!

Until then, here’s an interactive cheatsheet:
[es6cheatsheet.com](https://es6cheatsheet.com/). It uses runnable code
samples to compare the ES5 way with the ES6 way so you can brush up
quickly.

To give you a taste of React alternatives, we’ll check out **Preact**,
**Inferno**, and **Vue** in the fractal tree example. They’re
component-based UI frameworks similar to React. Preact is tiny, Inferno
is fast, and Vue is about as popular as React.

-----

Let’s talk about how React and D3 fit together. If there’s just one
chapter you should read, this is the one. It explains how things work
and fit together. Takes you from beginning to being able to figure out
the rest on your own.

Although you should still play with the bigger examples. They’re fun.

This section has five chapters:

  - [A quick intro to
    D3](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6887710)
  - [How React makes D3
    easier](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6887721#basic-approach)
  - [When should you use an existing library? Which
    one?](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6887723#existing-libraries)
  - [Quickly integrate any D3 code in your React project with Blackbox
    Components](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6887730#blackbox-components)
  - [Build scalable dataviz components with full
    integration](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6887740#full-feature-integration)
  - [Handling state in your React
    app](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6887771#state-handling-architecture)
  - [Structuring your React
    App](https://swizec1.teachable.com/courses/react-for-data-visualization/lectures/6887772#structuring-your-app)
