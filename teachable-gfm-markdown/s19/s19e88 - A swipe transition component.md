
## Swipe transition

Our goal is to build a declarative component fully controlled by props.
We pass in the `x` coordinate and the coordinate figures out the rest.

We shouldn’t care about the transition or how long it takes. That’s up
to the component. We just change `x`.

You can see it in action [on CodeSandbox,
here](https://codesandbox.io/s/618mr9r6nr). Tweak params, see what
happens. Follow along as I explain how it works.

![Ball on the left before you
click](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/ball-swipe-left.png)
![Ball on the right before you
click](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/ball-swipe-right.png)
