
### Part 1: Rendering our marbles

Our marbles render on Canvas using Konva. Each marble is its own sprite
rendered as a Konva element. This makes it easier to implement user and
marble interactions.

Rendering happens in 3 components:

  - `App` holds everything together
  - `MarbleList` renders a list of marbles
  - `Marble` renders an individual marble

We’re also using 2 MobX stores:

  - `Sprite` to load the marble sprite and define coordinates within
  - `Physics` as our physics engine

`Sprite` and `Physics` are hold almost all of our game logic. A bit of
drag & drop logic goes in the `Marble` component. Other than that, all
our components are presentational. They get props and render stuff.

Let’s start with `App` and work our way down.

#### App

Our `App` component doesn’t do much. It imports MobX stores, triggers
sprite loading, and starts the game loop.

``` javascript
// src/components/App.js

import React, { Component } from 'react';
import { Provider as MobXProvider, observer } from 'mobx-react';

import Physics from '../logic/Physics';
import Sprite from '../logic/Sprite';
import MarbleList from './MarbleList';

@observer
class App extends Component {
    componentDidMount() {
        Sprite.loadSprite(() => Physics.startGameLoop());
    }

    render() {
        return (
            <div className="App">
                <div className="App-header">
                    <h2>Elastic collisions</h2>
                    <p>Rendered on canvas, built with React and Konva</p>
                </div>
                <div className="App-intro">
                    <MobXProvider physics={Physics} sprite={Sprite}>
                        <MarbleList />
                    </MobXProvider>
                </div>
            </div>
        );
    }
}

export default App;
```

We import our dependencies: React itself, a `MobXProvider` that’s
similar to the Redux provider (puts stuff in react context), both of our
MobX stores which export singleton instances, and the main `MarbleList`
component.

`App` itself is a full featured component that initiates sprite loading
in `componentDidMount` and calls `startGameLoop` when the sprite is
ready. We know the sprite is ready because it calls a callback. You’ll
see how that works in a bit.

The `render` method outputs some descriptive text and the `MarbleList`
component wrapped in a `MobXProvider`. The provider puts instances of
our stores – `sprite` and `physics` – in React context.

This makes them available to all child components via the `inject`
decorator.

#### MarbleList

`MarbleList` is an important component that renders the whole game, yet
it can still be small and functional. Every prop it needs comes from our
two stores.

Like this:

``` javascript
// src/components/MarbleList.js

import React from 'react';
import { inject, observer } from 'mobx-react';
import { Stage, Layer, Group } from 'react-konva';

import Marble from './Marble';

const MarbleList = inject('physics', 'sprite')(observer(({ physics, sprite }) => {
    const { width, height, marbles } = physics;
    const { marbleTypes } = sprite;

    return (
        <Stage width={width} height={height}>
            <Layer>
                <Group>
                    {marbles.map(({ x, y, id }, i) => (
                        <Marble x={x}
                                y={y}
                                type={marbleTypes[i%marbleTypes.length]}
                                draggable="true"
                                id={id}
                                key={`marble-${id}`} />
                    ))}
                </Group>
            </Layer>
        </Stage>
    );
}));

export default MarbleList;
```

We import dependencies and create a `MarbleList` component. Instead of
decorators, we’re using with functional composition.

This shows you that MobX *can* work without decorators, but there’s no
deep reason behind this choice. Over time, I’ve developed a preference
for composition for functional components and decorators for class-based
components.

`inject` takes values out of context and puts them in component props.
`observer` declares that our component observes those props and reacts
to them.

It’s generally a good idea to use both `inject` and `observer` together.
I have yet to find a case where you need just one or the other.

The rendering itself takes values out of our stores and returns a Konva
`Stage` with a single `Layer`, which contains a `Group`. Inside this
group is our list of marbles.

Each marble gets a position, a `type` that defines how it looks, an
`id`, and a `key`. We set `draggable` to `true` so Konva knows that this
element is draggable.

Yes, that means we get draggability on an HTML5 Canvas without any extra
effort. I like that.

#### Marble

Each `Marble` component renders a single marble and handles dragging and
dropping. That’s how you “shoot” marbles.

Dragging and dropping creates a vector that accelerates, or shoots, the
marble in a certain direction with a certain speed. Putting this logic
in the component itself makes sense because the rest of our game only
cares about that final vector.

The Marble component looks like this:

``` javascript
// src/components/Marble.js

import React, { Component } from 'react';
import { Circle } from 'react-konva';
import { inject, observer } from 'mobx-react';

@inject('physics', 'sprite') @observer
class Marble extends Component {
    onDragStart = () => {
        // set drag starting position
    }

    onDragMove = () => {
        // update marble position
    }

    onDragEnd = () => {
        // shoot the marble
    }

    render() {
        const { sprite, type, draggable, id, physics } = this.props;
        const MarbleDefinitions = sprite.marbleDefinitions;
        const { x, y, r } = physics.marbles[id];

        return (
            <Circle x={x} y={y} radius={r}
                    fillPatternImage={sprite.sprite}
                    fillPatternOffset={MarbleDefinitions[type]}
                    fillPatternScale={{ x: r*2/111, y: r*2/111 }}
                    shadowColor={MarbleDefinitions[type].c}
                    shadowBlur="15"
                    shadowOpacity="1"
                    draggable={draggable}
                    onDragStart={this.onDragStart}
                    onDragEnd={this.onDragEnd}
                    onDragMove={this.onDragMove}
                    ref="circle"
                    />
        );
    }
}

export default Marble;
```

We `@inject` both stores into our component and make it an `@observer`.
The `render` method takes values out of our stores and renders a Konva
`Circle`. The circle uses a chunk of our sprite as its background, has a
colorful shadow, and has a bunch of drag callbacks.

Those callbacks make our game playable.

In `onDragStart`, we store the starting position of the dragged marble.
In `onDragMove`, we update the marble’s position in the store, which
makes it possible for other marbles to bounce off of ours while it’s
moving, and in `onDragEnd`, we shoot the marble.

Shoot direction depends on how we dragged. That’s why we need the
starting positions.

Drag callbacks double as MobX actions. Makes our code simpler. Instead
of specifying an extra `@action` in the MobX store, we manipulate the
values directly.

MobX makes this okay. It keeps everything in sync and our state easy to
understand. MobX even batches value changes before triggering
re-renders.

The code inside those callbacks is pretty mathsy.

``` javascript
// src/components/Marble.js

class Marble extends Component {
    onDragStart = () => {
        const { physics, id } = this.props;

        this.setState({
            origX: physics.marbles[id].x,
            origY: physics.marbles[id].y,
            startTime: new Date()
        });
    }

    onDragMove = () => {
        const { physics, id } = this.props;
        const { x, y } = this.refs.circle.attrs;

        physics.marbles[id].x = x;
        physics.marbles[id].y = y;
    }

    onDragEnd = () => {
        const { physics } = this.props,
              circle = this.refs.circle,
              { origX, origY } = this.state,
              { x, y } = circle.attrs;


        const delta_t = new Date() - this.state.startTime,
              dist = (x - origX) ** 2 + (y - origY) ** 2,
              v = Math.sqrt(dist)/(delta_t/16); // distance per frame (= 16ms)

        physics.shoot({
           x: x,
           y: y,
           vx: (x - origX)/(v/3), // /3 is a speedup factor
           vy: (y - origY)/(v/3)
           }, this.props.id);
    }

    // ...
}
```

In `onDragStart`, we store original coordinates and start time in local
state. These are temporary values that nothing outside this user action
cares about. Local state makes sense.

We’ll use them to determine how far the user dragged our marble.

In `onDragMove` we update the MobX store with new coordinates for this
particular marble. You might think we’re messing with mutable state
here, and we *might* be, but these are MobX observables. They’re wrapped
in setters that ensure everything is kept in sync, changes logged,
observers notified, etc.

`onDragEnd` shoots the marble. We calculate drag speed and direction,
then we call the `shoot()` action on the `physics` store.

The math we’re doing is called [euclidean
distance](https://en.wikipedia.org/wiki/Euclidean_distance) by the way.
Distance between two points is the root of the sum of squares of
distance on each axis.

#### Sprite store

Now that we know how rendering works, we need to load our sprite. It’s
an icon set I bought online. Can’t remember where or who from.

Here’s what it looks like:

![Marbles
sprite](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/monster-marbles-sprite-sheets.jpg)

To use this sprite, we need two things:

1.  A way to tell where on the image each marble lies
2.  A MobX store that loads the image into memory

The first is a `MarbleDefinitions` dictionary. We used it in `Marble`
component’s render method. If you’re playing along, you should copy
paste this. Too much typing :)

``` javascript
// src/logic/Sprite.js

const MarbleDefinitions = {
    dino: { x: -222, y: -177, c: '#8664d5' },
    redHeart: { x: -222, y: -299, c: '#e47178' },
    sun: { x: -222, y: -420, c: '#5c96ac' },

    yellowHeart: { x: -400, y: -177, c: '#c8b405' },
    mouse: { x: -400, y: -299, c: '#7d7e82' },
    pumpkin: { x: -400, y: -420, c: '#fa9801' },

    frog: { x: -576, y: -177, c: '#98b42b' },
    moon: { x: -575, y: -299, c: '#b20717' },
    bear: { x: -576, y: -421, c: '#a88534' }
};

export { MarbleDefinitions };
```

Each type of marble has a name, a coordinate, and a color. The
coordinate tells us where on the sprite image it is, and the color helps
us create a nice shadow.

All values painstakingly assembled by hand. You’re welcome. 😌

The MobX store that loads our sprite into memory and helps us use it
looks like this:

{caption: “Sprite store”, line-numbers: false}

``` javascript
// src/logic/Sprite.js

import { observable, action, computed } from "mobx";
import MarbleSprite from "../monster-marbles-sprite-sheets.jpg";

class Sprite {
  @observable sprite = null;

  @action loadSprite(callback = () => null) {
    const sprite = new Image();
    sprite.src = MarbleSprite;

    sprite.onload = () => {
      this.sprite = sprite;

      callback();
    };
  }

  @computed get marbleTypes() {
    return Object.keys(MarbleDefinitions);
  }

  @computed get marbleDefinitions() {
    return MarbleDefinitions;
  }
}

export default new Sprite();
```

A MobX store is a JavaScript object. It has `@observable` values,
`@actions`, and `@computed` getters. That’s all there is to it.

No complicated reducers and action generators. Just JavaScript functions
and properties. There’s plenty going on behind the scenes, but we don’t
have to think about it.

That’s why I like MobX more than Redux. Feels easier to use 🤫

In the `Sprite` store, we have an `@observable sprite`. Changing this
value triggers a re-render in al `@observer` components that rely on it.
In our case that’s every marble.

Then we have a `loadSprite` action. It creates a new `Image` object and
loads the sprite. After the image loads, we set `this.sprite`.

The `@computed` getters make it easier to access `MarbleDefinitions`.
`marbleTypes` gives us a list of available types of marbles and
`marbleDefinitions` returns the definitions object.

Running your code won’t work just yet. We need the physics store first
because it defines marble positions.
