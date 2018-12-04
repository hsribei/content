
## 2 Redux Reducers

With the actions firing and the drawing done, it’s time to look at the
business logic of our particle generator. We’ll get it done in just 33
lines of code and some change.

Well, it’s a bunch of change. But the 33 lines that make up
`CREATE_PARTICLES` and `TIME_TICK` changes are the most interesting. The
rest just flips various flags.

All our logic and physics goes in the reducer. [Dan Abramov
says](http://redux.js.org/docs/basics/Reducers.html) to think of
reducers as the function you’d put in `.reduce()`. Given a state and a
set of changes, how do I create the new state?

A “sum numbers” example would look like this:

``` javascript
let sum = [1,2,3,4].reduce((sum, n) => sum+n, 0);
```

For each number, take the previous sum and add the number.

Our particle generator is a more advanced version of the same concept:
Takes current application state, incorporates an action, and returns new
application state.

Start with a default state and some D3 random number helpers.

``` javascript
import { randomNormal } from "d3";

const Gravity = 0.5,
    randNormal = randomNormal(0.3, 2),
    randNormal2 = randomNormal(0.5, 1.8);

const initialState = {
    particles: [],
    particleIndex: 0,
    particlesPerTick: 30,
    svgWidth: 800,
    svgHeight: 600,
    isTickerStarted: false,
    generateParticles: false,
    mousePos: [null, null],
    lastFrameTime: null
};
```

Using D3’s `randomNormal` random number generator creates a better
random distribution than using JavaScript’s own `Math.random`. The rest
is a bunch of default state 👇

  - `particles` holds an array of particles to draw
  - `particleIndex` defines the ID of the next generated particle
  - `particlesPerTick` defines how many particles we create on each
    requestAnimationFrame
  - `svgWidth` is the width of our drawing area
  - `svgHeigh` is the height
  - `isTickerStarted` specifies whether the animation is running
  - `generateParticles` turns particle generation on and off
  - `mousePos` defines the origination point for new particles
  - `lastFrameTime` helps us compensate for dropped frames

To manipulate all this state, we use two reducers and manually combine
them. Redux does come with a `combineReducers` function, but I wanted to
keep our state flat and that doesn’t fit `combineReducers`’s view of how
life should work.

``` javascript
// src/reducers/index.js

// Manually combineReducers
export default function(state = initialState, action) {
    return {
        ...appReducer(state, action),
        ...particlesReducer(state, action)
    };
}
```

This is our reducer. It takes current `state`, sets it to `initialState`
if undefined, and an action. To create new state, it spreads the object
returned from `appReducer` and from `particlesReducer` into a new
object. You can combine as many reducers as you want in this way.

The usual `combineReducers` approach leads to nested hierarchical state.
That often works great, but I wanted to keep our state flat.

Lesson here is that there are no rules. You can make your reducers
whatever you want. Combine them whichever way fits your use case. As
long as you take a state object and an action and return a new state
object.

`appReducer` will handle the constants and booleans and drive the
metadata for our animation. `particlesReducer` will do the hard work of
generating and animating particles.

### Driving the basics with appReducer

Our `appReducer` handles the boring actions with a big switch statement.
These are common in the Redux world. They help us decide what to do
based on action type.

``` javascript
// src/reducers/index.js
function appReducer(state, action) {
    switch (action.type) {
        case "TICKER_STARTED":
            return Object.assign({}, state, {
                isTickerStarted: true,
                lastFrameTime: new Date()
            });
        case "START_PARTICLES":
            return Object.assign({}, state, {
                generateParticles: true
            });
        case "STOP_PARTICLES":
            return Object.assign({}, state, {
                generateParticles: false
            });
        case "UPDATE_MOUSE_POS":
            return Object.assign({}, state, {
                mousePos: [action.x, action.y]
            });
        case "RESIZE_SCREEN":
            return Object.assign({}, state, {
                svgWidth: action.width,
                svgHeight: action.height
            });
        default:
            return state;
    }
}
```

Gotta love that boilerplate 😛

Even though we’re only changing values of boolean flags and two-digit
arrays, *we have to create a new state*. Redux relies on application
state being immutable.

Well, JavaScript doesn’t have real immutability. We pretend and make
sure to never change state without making a new copy first. There are
libraries that give you proper immutable data structures, but that’s a
whole different course.

We use `Object.assign({}, ...` to create a new empty object, fill it
with the current state, then overwrite specific values with new ones.
This is fast enough even with large state trees thanks to modern
JavaScript engines.

Note that when a reducer doesn’t recognize an action, it has to return
the same state it received. Otherwise you end up wiping state. 😅

So that’s the boilerplatey state updates. Manages starting and stopping
the animation, flipping the particle generation switch, and resizing our
viewport.

The fun stuff happens in `particleReducer`.

### Driving particles with particleReducer

Our particles live in an array. Each particle has an id, a position, and
a vector. That tells us where to draw the particle and how to move it to
its future position.

On each tick of the animation we have to:

1.  Generate new particles
2.  Remove particles outside the viewport
3.  Move every particle by its vector

We can do all that in one big reducer, like this:

``` javascript
// src/reducers/index.js
function particlesReducer(state, action) {
    switch (action.type) {
        case "TIME_TICK":
            let {
                    svgWidth,
                    svgHeight,
                    lastFrameTime,
                    generateParticles,
                    particlesPerTick,
                    particleIndex,
                    mousePos
                } = state,
                newFrameTime = new Date(),
                multiplier = (newFrameTime - lastFrameTime) / (1000 / 60),
                newParticles = state.particles.slice(0);

            if (generateParticles) {
                for (let i = 0; i < particlesPerTick; i++) {
                    let particle = {
                        id: state.particleIndex + i,
                        x: mousePos[0],
                        y: mousePos[1]
                    };

                    particle.vector = [
                        particle.id % 2 ? -randNormal() : randNormal(),
                        -randNormal2() * 3.3
                    ];

                    newParticles.unshift(particle);
                }

                particleIndex = particleIndex + particlesPerTick + 1;
            }

            let movedParticles = newParticles
                .filter(p => {
                    return !(p.y > svgHeight || p.x < 0 || p.x > svgWidth);
                })
                .map(p => {
                    let [vx, vy] = p.vector;
                    p.x += vx * multiplier;
                    p.y += vy * multiplier;
                    p.vector[1] += Gravity * multiplier;
                    return p;
                });

            return {
                particles: movedParticles,
                lastFrameTime: new Date(),
                particleIndex
            };
        default:
            return {
                particles: state.particles,
                lastFrameTime: state.lastFrameTime,
                particleIndex: state.particleIndex
            };
    }
}
```

That’s a lot of code, I know. Let me explain :)

The first part takes important values out of `state`, calculates the
dropped frame multiplier, and makes a new copy of the particles array
with `.slice(0)`. That was the fastest way I could find.

Then we generate new particles.

We loop through `particlesPerTick` particles, create them at `mousePos`
coordinates, and insert at the beginning of the array. In my tests that
performed best. Particles get random movement vectors.

This randomness is a Redux faux pas. Reducers are supposed to be
functionally pure: produce the same result every time they are called
with the same argument values. Randomness is impure.

We don’t need our particle vectors to be deterministic, so I think this
is fine. Let’s say our universe is stochastic instead 😄

{aside} Stochastic means that our universe/physic simulation is governed
by probabilities. You can still model such a universe and reason about
its behavior. A lot of real world physics is stochastic in nature.
{/aside}

We now have an array full of old and new particles. We remove all
out-of-bounds particles with a `filter`, then walk through what’s left
to move each particle by its vector.

To simulate gravity, we update vectors’ vertical component using our
`Gravity` constant. That makes particles fall down faster and faster
creating a nice parabola.

Our reducer is done. Our particle generator works. Our thing animates
smoothly. \\o/
