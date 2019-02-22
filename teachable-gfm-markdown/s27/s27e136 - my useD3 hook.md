
## my useD3 hook

Remember when we talked about D3 blackbox rendering? I built a hook for
that so you don’t have to mess around with HOCs anymore :)

Read the full docs at [d3blackbox.com](https://d3blackbox.com/)

It works as a combination of `useRef` and `useEffect`. Hooks into
component re-renders, gives you control of the anchor element, and
re-runs your D3 render function on every component render.

You use it like this:

``` javascript
import { useD3 } from "d3blackbox";
function renderSomeD3(anchor) {
    d3.select(anchor);
    // ...
}

const MyD3Component = ({ x, y }) => {
    const refAnchor = useD3(anchor => renderSomeD3(anchor));
    return <g ref={refAnchor} transform={`translate(${x}, ${y})`} />;
};
```

You’ll see how this works in more detail when we refactor the big
example to hooks. We’ll use `useD3` to build axes.
