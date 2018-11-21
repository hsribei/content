
## 1 Container component

Containers are React components that talk to the Redux data store.

You can think of presentation components as templates that render stuff
and containers as smart-ish views that talk to controllers. Or maybe
they’re the controllers.

Sometimes it’s hard to tell. In theory presentation components render
and don’t think, containers communicate and don’t render. Redux reducers
and actions do the thinking.

I’m not sure this separation is necessary in small projects.

Maintaining it can be awkward and sometimes cumbersome in mid-size
projects, but I’m sure it makes total sense at Facebook scale. We’re
using it in this project because the community has decided that’s the
way to go.

We use the idiomatic `connect()` approach. Like this:

``` javascript
// src/containers/AppContainer.jsx

import { connect } from "react-redux";
import React, { Component } from "react";
import * as d3 from "d3";

import App from "../components";
import {
    tickTime,
    tickerStarted,
    startParticles,
    stopParticles,
    updateMousePos
} from "../actions";

class AppContainer extends Component {
    startTicker = () => {
        const { isTickerStarted } = this.props;

        if (!isTickerStarted) {
            console.log("Starting ticker");
            this.props.tickerStarted();
            d3.timer(this.props.tickTime);
        }
    };

    render() {
        const { svgWidth, svgHeight, particles } = this.props;

        return (
            <App
                svgWidth={svgWidth}
                svgHeight={svgHeight}
                particles={particles}
                startTicker={this.startTicker}
                startParticles={this.props.startParticles}
                stopParticles={this.props.stopParticles}
                updateMousePos={this.props.updateMousePos}
            />
        );
    }
}

const mapStateToProps = ({
    generateParticles,
    mousePos,
    particlesPerTick,
    isTickerStarted,
    svgWidth,
    svgHeight,
    particles
}) => ({
    generateParticles,
    mousePos,
    particlesPerTick,
    isTickerStarted,
    svgWidth,
    svgHeight,
    particles
});

const mapDispatchToProps = {
    tickTime,
    tickerStarted,
    startParticles,
    stopParticles,
    updateMousePos
};

export default connect(
    mapStateToProps,
    mapDispatchToProps
)(AppContainer);
```

I love the smell of boilerplate in the morning. 👃

We import dependencies and define `AppContainer` as a class-based React
`Component` so we have somewhere to put the D3 interval. The render
method outputs our `<App>` component using a bunch of props to pass
relevant actions and values.

The `startTicker` method is a callback we pass into App. It runs on
first click and starts the D3 interval if necessary. Each interval
iteration triggers the `tickTime` action.

### AppContainer talks to the store

``` javascript
// src/containers/AppContainer.jsx

const mapStateToProps = ({
    generateParticles,
    mousePos,
    particlesPerTick,
    isTickerStarted,
    svgWidth,
    svgHeight,
    particles
}) => ({
    generateParticles,
    mousePos,
    particlesPerTick,
    isTickerStarted,
    svgWidth,
    svgHeight,
    particles
});

const mapDispatchToProps = {
    tickTime,
    tickerStarted,
    startParticles,
    stopParticles,
    updateMousePos
};

export default connect(
    mapStateToProps,
    mapDispatchToProps
)(AppContainer);
```

We’re using the `connect()` idiom to connect our AppContainer to the
Redux store. It’s a higher order component that handles all the details
of connection to the store and staying in sync.

We pass two arguments into connect. This returns a higher order
component function, which we wrap around AppContainer.

The first argument is **mapStateToProps**. It accepts current state as
an argument, which we immediately deconstruct into interesting parts,
and returns a key:value dictionary. Each key becomes a component prop
with the corresponding value.

You’d often use this opportunity to run ad-hoc calculations, or combine
parts of state into single props. No need for that in our case, just
pass it through.

#### Dispatching actions

**mapDispatchToProps** is a dictionary that maps props to actions. Each
prop turns into an action generator wrapped in a `store.dispatch()`
call. To fire an action inside a component we just call the function in
that prop.

But Swiz, we’re not writing key:value dictionaries, we’re just listing
stuff\!

That’s a syntax supported in most modern JavaScript environments, called
object literal property value shorthand. Our build system expands that
`mapDispatchToProps` dictionary into something like this:

``` javascript
const mapDispatchToProps = {
    tickTime: tickTime,
    tickerStarted: tickerStarted,
    startParticles: startParticles,
    stopParticles: stopParticles,
    updateMousePos: updateMousePos
};
```

And you thought previous code had a lot of boilerplate … imagine if this
was how you’d do it in real life 😛

`connect` wraps each of these action generators in `store.dispatch()`
calls. You can pass the resulting function into any component and fire
actions by calling that method.

#### The Redux loop

To make a change therefore, a Redux loop unfolds:

1.  Call our action triggerer, passed in through props
2.  Calls the generator, gets a `{type: ...}` object
3.  Dispatches that object on the store
4.  Redux calls the reducer
5.  Reducer creates new state
6.  Store updates triggering React’s engine to flow updates through the
    props

So that’s the container. 71 lines of boilerplate pretty code.

The remaining piece of the puzzle is our reducer. Two reducers in fact.
