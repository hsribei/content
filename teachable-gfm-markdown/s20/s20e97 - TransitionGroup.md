
#### TransitionGroup

React TransitionGroup gives us coordinated control over a set of
transitionable components. Each Letter is going to be a `<Transition>`,
you’ll see.

We need TransitionGroup to gain declarative control over the enter/exit
cycle. Transition components can handle transitions themselves, but they
need an `in` prop to say whether they’re in or out of the visualization.

Flip from `false` to `true`, run an enter transition.

`true` to `false`, run an exit transition.

We can make this change from within our component, of course. When
responding to user events for example. In our case we need that control
to come from outside based on which letters exist in the `alphabet`
array.

`TransitionGroup` handles that for us. It automatically passes the
correct `in` prop to its children based on who is and isn’t being
rendered.

As an added bonus, we can use TransitionGroup to set a bunch of default
parameters for child Transitions. Whether to use `enter` animations,
`exit` animations, stuff like that. You can read [a full list in the
docs](https://github.com/reactjs/react-transition-group).
