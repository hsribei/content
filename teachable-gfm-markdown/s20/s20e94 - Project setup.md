
### Project setup

To get started you’ll need a project. Either start one with
`create-react-app` or in CodeSandbox. Either will work.

You’ll need a base App component that renders an SVG with an
`<Alphabet>` child. Our component is self-contained so that’s all you
need.

Something like this 👇

``` javascript
import Alphabet from './components/Alphabet`;

const App = () => (
  <svg width="100%" height="600">
    <Alphabet x={32} y={300} />
  </svg>
)
```

I follow the convention of putting components in a `src/components`
directory. You don’t have to.

Remember to install dependencies: d3 and react-transition-group
