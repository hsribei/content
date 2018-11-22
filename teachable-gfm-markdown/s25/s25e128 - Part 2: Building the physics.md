
### Part 2: Building the physics

Our whole physics engine fits into a single MobX store. It contains the
collision detection, marble movement calculations, and drives the game
loop itself.

The general approach goes like this:

1.  Have an observable array of marbles
2.  Run a `simulationStep` on each `requestAnimationFrame` using
    `d3.timer`
3.  Change marble positions and speed
4.  MobX observables and observers trigger re-renders of marbles that
    move

The [whole Physics
store](https://github.com/Swizec/declarative-canvas-react-konva/blob/master/src/logic/Physics.js)
is some 120 lines of code. We’ll go slow. Here’s the skeleton:

{caption: “Physics skeleton”, line-numbers: false}

``` javascript
// src/logic/Physics.js

class Physics {
  @observable MarbleR = 25;
  @observable width = 800;
  @observable height = 600;
  @observable marbles = [];
  timer = null;

  @computed get initialPositions() {}

  @action startGameLoop() {}

  @action simulationStep() {}

  @action shoot({ x, y, vx, vy }, i) {}
}
```

We have four observable properties, a `timer`, a `@computed` property
for initial positions, and 3 actions. `startGameLoop` starts our game,
`simulationStep` holds the main logic, and `shoot` shoots a particular
marble.

Let’s walk through.

#### initialPositions

{caption: “initialPositions function”, line-numbers: false}

``` javascript
// src/logic/Physics.js
class Physics {
  // ..
  @computed get initialPositions() {
    const { width, height, MarbleR } = this,
      center = width / 2;

    const lines = 4,
      maxY = 200;

    let marbles = range(lines, 0, -1)
      .map(y => {
        if (y === lines)
          return [{ x: center, y: maxY, vx: 0, vy: 0, r: this.MarbleR }];

        const left = center - y * (MarbleR + 5),
          right = center + y * (MarbleR + 5);

        return range(left, right, MarbleR * 2 + 5).map(x => ({
          x: x,
          y: maxY - y * (MarbleR * 2 + 5),
          vx: 0,
          vy: 0,
          r: this.MarbleR
        }));
      })
      .reduce((acc, pos) => acc.concat(pos), []);

    marbles = [].concat(marbles, {
      x: width / 2,
      y: height - 150,
      vx: 0,
      vy: 0,
      r: this.MarbleR
    });

    marbles.forEach((m, i) => (marbles[i].id = i));

    return marbles;
  }
  // ..
}
```

Believe it or not, this is like one of those *“Arrange things in a
triangle”* puzzles you’d see in an old Learn How To Program book. Or a
whiteboard interview.

It took me 3 hours to build. Easy to get wrong, fiddly to implement.

We start with a `range` of numbers. From `lines` to `0` in descending
order. We iterate through this list of rows and change each into a list
of marbles.

4 marbles in the first row, 3 in the next, all the way down to 1 in last
row.

For each row, we calculate how much space we have on the `left` and
`right` of the center and make a `range` of horizontal positions from
`left` to `right` with a step of “1 marble size”. Using these positions
and the known row, we create marbles as needed.

We use a `.reduce` to flatten nested arrays and add the last marble.
That’s a corner case I couldn’t solve elegantly, but I’m sure it’s
possible.

In the end, we add an `id` to each marble. We’re using index as the id,
that’s true, but that still ensures we use consistent values throughout
our app. Positions in the array may change.

#### shoot and startGameLoop

{caption: “shoot and startGameLoop functions”, line-numbers: false}

``` javascript
// src/logic/Physics.js
class Physics {
  // ...

  @action startGameLoop() {
    this.marbles = this.initialPositions;

    this.timer = timer(() => this.simulationStep());
  }

  // ...

  @action shoot({ x, y, vx, vy }, i) {
    const maxSpeed = 20;

    this.marbles[i].x = x;
    this.marbles[i].y = y;
    this.marbles[i].vx = vx < maxSpeed ? vx : maxSpeed;
    this.marbles[i].vy = vy < maxSpeed ? vy : maxSpeed;
  }
}
```

`shoot` and `startGameLoop` are the simplest functions in our physics
engine. `startGameLoop` gets the initial `marbles` array and starts a D3
timer. `shoot` updates a specific marble’s coordinates and speed vector.

👌
