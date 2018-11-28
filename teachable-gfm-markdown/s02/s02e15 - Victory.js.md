
## Victory

> React.js components for modular charting and data visualization

[![Victory.js
logo](https://raw.githubusercontent.com/hsribei/react-d3js-es6-ebook/teachable-only/manuscript/resources/images/2018/victoryjs.jpg)](http://formidable.com/open-source/victory/)

Victory offers low level components for basic charting and reimplements
a lot of D3’s API. Great when you need to create basic charts without a
lot of customization. Supports React Native.

Here’s what it takes to implement a Barchart using Victory.js. [You can
try it on
CodeSandbox](https://codesandbox.io/s/3v3q013x36)

<iframe src="https://codesandbox.io/embed/3v3q013x36?codemirror=1&amp;view=split" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin">

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
    <h1>Victory basic demo</h1>
    <VictoryChart domainPadding={20}>
      <VictoryBar data={data} x="quarter" y="earnings" />
    </VictoryChart>
  </div>
);
```

Create some fake data, render a `<VictoryChart>` rendering area, add a
`<VictoryBar>` component, give it data and axis keys. Quick and easy.

My favorite feature of Victory is that components use fake random data
until you pass your own. Means you always know what to expect.
