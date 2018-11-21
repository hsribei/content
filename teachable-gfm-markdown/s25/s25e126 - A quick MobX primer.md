
### A quick MobX primer

Explaining MobX in detail is beyond the scope of this book. You can
learn it by osmosis as you follow the code in our billiards example.

That said, here’s a quick rundown of the concepts we’re using.

MobX is based on reactive programming. There are values that are
observable and functions that react when those values change. MobX
ensures only the minimal possible set of observers is triggered on every
change.

So, we have:

`@observable` – a property whose changes observers subscribe to
`@observer` – a component whose `render()` method observes values
`@computed` – a method whose value can be fully derived from observables
`@action` – a method that changes state, analogous to a Redux reducer
`@inject` – a decorator that injects global stores into a component’s
props

That’s all you need to know. Once your component is an `@observer`, you
never have to worry about *what* it’s observing. MobX ensures it reacts
to changes in values used during rendering.

Making your component an observer and injecting the global store is the
same as using `connect` in Redux. It gives your component access to your
state, and it triggers a re-render when something changes.

Importantly, it *doesn’t* trigger a re-render when something that the
component isn’t using changes. That little tidbit is what makes many
other reactive libraries difficult to use.
