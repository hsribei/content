
## useMemo is your new best dataviz friend

My favorite hook for making React and D3 work together is `useMemo`.
It’s like a combination of `useEffect` and `useState`.

Remember how the rest of this course focused on syncing D3’s internal
state with React’s internal state and complications around large
datasets and speeding up your D3 code to avoid recomputing on every
render?

All that goes away with `useMemo` – it memoizes values returned from a
function and recomputes them when particular props change. Think of it
like a cache.

Say you have a D3 linear scale. You want to update its range when your
component’s width changes.

``` javascript
function MyComponent({ data, width }) {
    const scale = useMemo(() => 
        d3.scaleLinear()
            .domain([0, 1])
            .range([0, width])
    ), [width])

    return <g> ... </g>
}
```

`useMemo` takes a function that returns a value to be memoized. In this
case that’s the linear scale.

You create the scale same way you always would. Initiate a scale, set
the domain, update the range. No fancypants trickery.

`useMemo`’s second argument works much like useEffect’s does: It tells
React which values to observe for change. When that value changes,
`useMemo` reruns your function and gets a new scale.

Don’t rely on `useMemo` running only once however. Memoization is meant
as a performance optimization, a hint if you will, not as a syntactical
guarantee of uniqueness.

And that’s exactly what we want. No more futzing around with
`getDerivedStateFromProps` and `this.state`. Just `useMemo` and leave
the rest to React. :v:
