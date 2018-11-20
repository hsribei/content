
# How to structure your app

Our app is built from components. You already know how components work.
Where it gets tricky is deciding where one component ends and another
beings.

One of the hardest problems in software engineering I’d say. Defining
the boundaries and interfaces between objects. Entire books have been
written on the subject.

You’ll learn most of it with experience. Just through trying different
approaches, seeing what works, and developing your taste. Like a chef
gets better at improvisation the more things they try. Or a musician.

To get you started, here’s a rule of thumb that I like to use: if you
have to use the word “and” to describe what your component does, then it
should become two components. If you build the same feature multiple
times, turn it into a component.

You can then remix components into larger components where it makes
sense, or you can keep them separate. It makes sense to combine when
multiple small components have to work together to build a big feature.

This architecture often mimics the design of your UI.

For example: our tech salary visualization is going to use 1 very top
level component, 5 major components, and a bunch of child components.

  - `App` is the very top level component; it keeps everything together
  - `Title` renders the dynamic title
  - `Description` renders the dynamic description
  - `Histogram` renders the histogram and has child components for the
    axis and histogram bars
  - `CountyMap` renders the choropleth map and uses child components for
    the counties
  - `Controls` renders the rows of buttons that let users explore our
    dataset

Most of these are specific to our use case, but `Histogram` and
`CountyMap` have potential to be used elsewhere. We’ll keep that in mind
when we build them.

`Histogram`, `CountyMap`, and `Controls` are going to have their own
folder inside `src/components/` to help us group major components with
their children. An `index.js` file will help with imports.

We’ll use a `Meta` folder for all our metadata components like `Title`
and `Description`. We don’t *have* to do this, but `import { Title,
Description } from './Meta'` looks better than separate imports for
related-but-different components. Namespacing, if you will.

We want to access every component with `import My Component from
'./MyComponent'` and render as `<MyComponent {...params} />`. Something
is wrong, if a parent component has to know details about the
implementation of a child component.

You can read more about these ideas by Googling [“leaky
abstractions”](https://en.wikipedia.org/wiki/Leaky_abstraction),
[“single responsibility
principle”](https://en.wikipedia.org/wiki/Single_responsibility_principle),
[“separation of
concerns”](https://en.wikipedia.org/wiki/Separation_of_concerns), and
[“structured
programming”](https://en.wikipedia.org/wiki/Structured_programming).
Books from the late 90’s and early 2000’s (when object-oriented
programming was The Future™) have the best curated info in my
experience.
