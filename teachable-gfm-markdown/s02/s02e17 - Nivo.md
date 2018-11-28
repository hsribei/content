
## Nivo

> nivo provides a rich set of dataviz components, built on top of the
> awesome d3 and Reactjs libraries.

[![Nivo
homepage](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/nivo.gif)](https://nivo.rocks/)

Nivo is another attempt to give you a set of basic charting components.
Comes with great interactive documentation, support for Canvas and API
rendering. Plenty of basic customization.

Here’s what it takes to implement a Barchart using Nivo. [You can try it
on
CodeSandbox](https://codesandbox.io/s/n1wwkvq24)

<iframe src="https://codesandbox.io/embed/n1wwkvq24?codemirror=1&amp;view=split" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin">

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
    <h1>Nivo basic demo</h1>
    <div style={{ height: "400px" }}>
      <ResponsiveBar data={data} keys={["earnings"]} indexBy="quarter" />
    </div>
  </div>
);
```

Least amount of effort\! You render a `<ResponsiveBar>` component, give
it data and some params, and Nivo handles the rest.

Wonderful\! But means you have to learn a whole new language of configs
and props that might make your hair stand on end. The documentation is
great and shows how everything works, but I found it difficult to know
which prop combinations are valid.
