
# Refactor your components with React Hooks

Hooks launched with great fanfare and community excitement in February
2019. A whole new way to write React components.

Not a fundamental change, the React team would say, but a mind shift
after all. A change in how you think about state, side effects, and
component structure. You might not like hooks at first. With a little
practice they’re going to feel more natural than the component lifecycle
model you’re used to now.

Hooks exist to:

  - help you write less code for little bits of logic
  - help you move logic out of your components
  - help you reuse logic between components

Some say it removes complexity around the `this` keyword in JavaScript
and I’m not sure that it does. Yes, there’s no more `this` because
everything is a function, that’s true. But you replace that with passing
a lot of function arguments around and being careful about function
scope.

My favorite React Hooks magic trick is that you can and should write
your own. Reusable or not, a custom hook almost always cleans up your
code.

Oh and good news\! Hooks are backwards compatible. You can write new
stuff with hooks and keep your old stuff unchanged. :ok\_hand:

In this section we’re going to look at the core React hooks, talk about
the most important hook for dataviz, then refactor our big example from
earlier to use hooks.

You’ll need to upgrade React to at least version `16.8.0` to use hooks.
