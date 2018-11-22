
## Step 1: Prep App.js

You know the drill, don’t you? Import some stuff, add it to the
`render()` method in the `App` component.

{format: javascript, line-numbers: false, caption: “Histogram imports”}

    // src/App.js
    import _ from 'lodash';
    
    // markua-start-insert
    import './style.css';
    // markua-end-insert
    
    import Preloader from './components/Preloader';
    import { loadAllData } from './DataHandling';
    
    import CountyMap from './components/CountyMap';
    // markua-start-insert
    import Histogram from './components/Histogram';
    // markua-end-insert

We import `style.css` and the `Histogram` component. That’s what I love
about Webpack - you can import CSS in JavaScript. We got the setup with
`create-react-app`.

There are competing schools of thought about styling React apps. Some
say each component should come with its own CSS files, some think it
should be in large per-app CSS files, many think CSS-in-JS is the way to
go.

Personally I like to use CSS for general cross-component styling and
styled-components for more specific styles. We’re using CSS in this
project because it works and means we don’t have to learn yet another
dependency.

After the imports, we can render our `Histogram` in the `App` component.

    // src/App.js
    // ...
    render() {
        // ...
        return (
            <div className="App container">
                <h1>Loaded {this.state.techSalaries.length} salaries</h1>
                <svg width="1100" height="500">
                    <CountyMap usTopoJson={this.state.usTopoJson}
                               USstateNames={this.state.USstateNames}
                               values={countyValues}
                               x={0}
                               y={0}
                               width={500}
                               height={500}
                               zoom={zoom} />
                    // markua-start-insert
                    <Histogram bins={10}
                               width={500}
                               height={500}
                               x="500"
                               y="10"
                               data={filteredSalaries}
                               axisMargin={83}
                               bottomMargin={5}
                               value={d => d.base_salary} />
                    // markua-end-insert
                </svg>
            </div>
        );
    }

We render the `Histogram` component with a bunch of props. They specify
the dimensions we want, positioning, and pass data to the component.
We’re using `filteredSalaries` even though we haven’t set up any
filtering yet. One less line of code to change later 👌

That’s it. `App` is ready to render our `Histogram`.

You should now see an error about missing files. That’s normal.
