
## Basic architecture

![Unidirectional data
flow](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/unidirectionalflow.png)

We’re using a unidirectional data flow architecture with a single source
of truth. That means you always know what to expect. Think of your app
as a giant circle.

Data goes from your source of truth into your components. Events go from
your components into your source of truth. All in one direction

![The basic
architecture](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/architecture.png)

Our main App component holds state for your entire application. Anything
that multiple components should be aware of lives here. This state flows
down the hierarchy via props. Changes happen via callbacks, also passed
down through props.

Like this 👇

  - The Main Component – `App` – holds the truth
  - Truth flows down through props
  - Child components react to user events
  - They announce changes using callbacks
  - The Main Component updates its truth
  - Updates flow back down the chain
  - UI updates through re-renders

This looks roundabout, but it’s amazing. Far better than worrying about
parts of the UI growing out of sync with the rest of your app. I could
talk your ear off with debugging horror stories, but I’m nice, so I
won’t.

When a user clicks one of our controls, a `Toggle`, it invokes a
callback. This in turn invokes a callback on `ControlRow`, which invokes
a callback on `Controls`, which invokes a callback on `App`.

![Callback
chain](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/architecture_callbacks.png)

With each hop, the nature of our callback changes. `Toggle` tells
`ControlRow` which entry was toggled, `ControlRow` tells `Controls` how
to update the data filter function, and `Controls` gives `App` a
composite filter built from all the controls. You’ll see how that works
in the next chapter.

All you have to remember right now is that callbacks evolve from passing
low-level information to high-level business logic. Starts with *“I was
clicked”* ends with *“Update visualization filter”*

When the final callback is invoked, `App` updates its repository of
truth – `this.state` – and communicates the change back down the chain
via props. No additional wiring needed on your part. React’s got you
covered.

![Data flows
down](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/architecture_dataflow.jpg)

You can think of it like calling functions with new arguments. Because
the functions – components – render the UI, your interface updates.

Because your components are well-made and rely on their props to render,
React’s engine can optimize these changes. It compares the new and old
component trees and decides which components to re-render and which to
leave alone.

Functional programming for HTML\! 😎

The functional programming concepts we’re relying on are called
[referential
transparency](https://en.wikipedia.org/wiki/Referential_transparency),
[idempotent functions](https://en.wikipedia.org/wiki/Idempotence), and
[functional purity](https://en.wikipedia.org/wiki/Pure_function). I
suggest Googling them if you want to learn the theory behind it all.
