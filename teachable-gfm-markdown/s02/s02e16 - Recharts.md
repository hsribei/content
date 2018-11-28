
## Recharts

> A composable charting library built on React components

[![Recharts
homepage](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/recharts.png)](http://recharts.org/)

Recharts is like a more colorful Victory. A pile of charting components,
some customization, loves animating everything by default.

Here’s what it takes to implement a Barchart using Recharts. [You can
try it on
CodeSandbox](https://codesandbox.io/s/mmkrjl7qxp)

<iframe src="https://codesandbox.io/embed/mmkrjl7qxp?codemirror=1&amp;view=split" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin">

</iframe>

``` javascript
const data = [
  { quarter: 1, earnings: 13000 },
  { quarter: 2, earnings: 16500 },
  { quarter: 3, earnings: 14250 },
  { quarter: 4, earnings: 19000 }
];

const App = () => (
  <div style={styles}>
    <h1>Recharts basic demo</h1>
    <BarChart width={500} height={300} data={data}>
      <XAxis dataKey="quarter" />
      <YAxis dataKey="earnings" />
      <Bar dataKey="earnings" />
    </BarChart>
  </div>
);
```

More involved than Victory, but same principle. Fake some data, render a
drawing area this time with `<BarChart>` and feed it some data. Inside
the `<BarChart>` render two axes, and a `<Bar>` for each entry.

Recharts hits a great balance of flexibility and ease … unless you don’t
like animation by default. Then you’re in trouble.
