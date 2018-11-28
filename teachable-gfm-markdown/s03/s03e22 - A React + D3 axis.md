
## A quick blackbox example - a React+D3 axis

Now let’s say we want to use that same axis code but as a React
component. The simplest way is to use a blackbox component approach like
this:

``` javascript
class Axis extends Component {
    gRef = React.createRef();
        
    componentDidMount() { this.d3render() }
    componentDidUpdate() { this.d3render() }

    d3render() {
        const scale = d3.scaleLinear()
                      .domain([0, 10])
                      .range([0, 200]);
        const axis = d3.axisBottom(scale);

        d3.select(this.gRef)
          .call(axis);  
    }

    render() {
        return <g transform="translate(10, 30)" ref={this.gRef} />
    }
}
```

So much code\! Worth it for the other benefits of using React in your
dataviz. You’ll see :)

We created an `Axis` component that extends React’s base `Component`
class. We can’t use functional components because we need lifecycle
hooks.

Our component has a `render` method. It returns a grouping element (`g`)
moved 10px to the right and 30px down using the `transform` attribute.
Same as before.

A React ref saved in `this.gRef` and passed into our `<g>` element with
`ref` lets us talk to the DOM node directly. We need this to hand over
rendering control to D3.

The `d3render` method looks familiar. It’s the same code we used in the
vanilla D3 example. Scale, axis, select, call. Only difference is that
instead of selecting `svg` and appending a `g` element, we select the
`g` element rendered by React and use that.

We use `componentDidUpdate` and `componentDidMount` to keep our render
up to date. Ensures that our axis re-renders every time React’s engine
decides to render our component.

That wasn’t so bad, was it?

[Try it out on
Codesandbox](https://codesandbox.io/s/3xy2jr1y5m).

<iframe src="https://codesandbox.io/embed/3xy2jr1y5m?codemirror=1&amp;view=split" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin">

</iframe>

You can make the axis more useful by getting positioning, scale, and
orientation from props. We’ll do that in our big project.

### Practical exercise

Try implementing those as an exercise. Make the axis more reusable with
some carefully placed props.

Here’s my solution, if you get stuck 👉
<https://codesandbox.io/s/5ywlj6jn4l>

<iframe src="https://codesandbox.io/embed/5ywlj6jn4l?codemirror=1&amp;view=split" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin">

</iframe>
