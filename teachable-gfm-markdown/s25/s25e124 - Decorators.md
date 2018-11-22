
### Decorators

Before we begin, let me tell you about decorators.

MobX embraces them to make its API easier to use. You can use MobX
without decorators, but decorators make it better.

A couple years ago, decorators got very close to becoming an official
spec, then got held back. I don’t know *why*, but they’re a great
feature whose syntax is unlikely to change. So even if MobX has to
change its implementation when decorators do land in the JavaScript
spec, you’re not likely to have to change anything.

You can think of decorators as function wrappers. Instead of code like
this:

{caption: “Decoratorless function wrapping”, line-numbers: false}

``` javascript
inject("store", ({ store }) => <div>A thing with {store.value}</div>);
```

You can write the same code like this:

{caption: “Function wrapping with decorators”, line-numbers: false}

``` javascript
@inject('store')
({ store }) => <div>A thing with {store.value}</div>
```

Not much of a difference, but it becomes better looking when you work
with classes or combine multiple decorators. That’s when they shine. No
more `})))}))` at the end of your functions.

By the way, `inject` is to MobX much like `connect` is to Redux. I’ll
explain in a bit.
