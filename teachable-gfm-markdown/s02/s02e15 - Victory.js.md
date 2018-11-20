
## Victory

> React.js components for modular charting and data visualization

[Victory.js
logo](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/victoryjs.gif)\](http://formidable.com/open-source/victory/)

Victory offers low level components for basic charting and reimplements
a lot of D3’s API. Great when you need to create basic charts without a
lot of customization. Supports React Native.

Here’s what it takes to implement a Barchart using Victory.js. [You can
try it on CodeSandbox](https://codesandbox.io/s/3v3q013x36)

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