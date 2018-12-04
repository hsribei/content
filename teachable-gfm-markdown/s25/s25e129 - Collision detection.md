
#### simulationStep – where collisions collision

Here comes the fun part. The one with our game loop.

There’s also a video explaining how this works 👉 [Watch it on
YouTube](https://www.youtube.com/watch?v=H84fmXjTElM). With hand-drawn
sketches that explain the math, and I think that’s neat.

``` javascript

@action simulationStep() {
    const { width, height, MarbleR } = this;

    const moveMarble = ({x, y, vx, vy, id}) => {
        let _vx = ((x+vx < MarbleR) ? -vx : (x+vx > width-MarbleR) ? -vx : vx)*.99,
            _vy = ((y+vy < MarbleR) ? -vy : (y+vy > height-MarbleR) ? -vy : vy)*.99;

        // nearest marble is a collision candidate
        const subdividedSpace = quadtree().extent([[-1, -1],
                                                   [this.width+1, this.height+1]])
                                          .x(d => d.x)
                                          .y(d => d.y)
                                          .addAll(this.marbles
                                                      .filter(m => id !== m.id)),
              candidate = subdividedSpace.find(x, y, MarbleR*2);

        if (candidate) {

            // borrowing @air_hadoken's implementation from here:
            // https://github.com/airhadoken/game_of_circles/blob/master/circles.js#L64
            const cx = candidate.x,
                  cy = candidate.y,
                  normx = cx - x,
                  normy = cy - y,
                  dist = (normx ** 2 + normy ** 2),
                  c = (_vx * normx + _vy * normy) / dist * 2.3;

            _vx = (_vx - c * normx)/2.3;
            _vy = (_vy - c * normy)/2.3;

            candidate.vx += -_vx;
            candidate.vy += -_vy;
            candidate.x += -_vx;
            candidate.y += -_vy;
        }

        return {
            x: x + _vx,
            y: y + _vy,
            vx: _vx,
            vy: _vy
        }
    };

    this.marbles.forEach((marble, i) => {
        const { x, y, vx, vy } = moveMarble(marble);

        this.marbles[i].x = x;
        this.marbles[i].y = y;
        this.marbles[i].vx = vx;
        this.marbles[i].vy = vy;
    });
}
```

That’s a lot of code 😅. Let’s break it down.

You can think of `simulationStep` as a function and a loop. At the
bottom, there is a `.forEach` that applies a `moveMarble` function to
each marble.

``` javascript
    this.marbles.forEach((marble, i) => {
        const { x, y, vx, vy } = moveMarble(marble);

        this.marbles[i].x = x;
        this.marbles[i].y = y;
        this.marbles[i].vx = vx;
        this.marbles[i].vy = vy;
    });
```

We iterate over the list of marbles, feed them into `moveMarble`, get
new properties, and save them in the main marbles array. MobX *should*
allows us to change these values inside `moveMarble` and let MobX
observables do the heavy lifting, but more explicit code is easier to
read.

##### moveMarble

`moveMarble` is itself a hairy function. Stuff happens in 3 steps:

1.  Handle collisions with walls
2.  Find collision with closest other marble
3.  Handle collision with marble

**Handling collisions with walls** happens in two lines of code. One per
axis.

``` javascript
let _vx = ((x+vx < MarbleR) ? -vx : (x+vx > width-MarbleR) ? -vx : vx)*.99,
    _vy = ((y+vy < MarbleR) ? -vy : (y+vy > height-MarbleR) ? -vy : vy)*.99;
```

Nested ternary expressions are kinda messy, but good enough. If a marble
is beyond any boundary, we reverse its direction. We *always* apply a
`.99` friction coefficient so that marbles slow down.

**Finding collisions** with the next closest marble happens using a
quadtree. Since we don’t have too many marbles, we can build a new
quadtree every time.

{aside} A quadtree is a way to subdivide space into areas. It lets us
answer the question of “What’s close enough to me to possibly touch me?”
without making too many position comparisons.

Checking every marble with every other marble produces 81 comparisons.
Versus 2 comparisons using a quadtree. {/aside}

``` javascript
// nearest marble is a collision candidate
const subdividedSpace = quadtree().extent([[-1, -1],
                                           [this.width+1, this.height+1]])
                                  .x(d => d.x)
                                  .y(d => d.y)
                                  .addAll(this.marbles
                                              .filter(m => id !== m.id)),
      candidate = subdividedSpace.find(x, y, MarbleR*2);
```

We’re using [`d3-quadtree`](https://github.com/d3/d3-quadtree) for the
quadtree implementation. It takes an `extent`, which tells it how big
our space is. An `x` and `y` accessor tells it how to get coordinates
out of our marble objects, and we use `addAll` to fill the quadtree with
marbles.

To avoid detecting each marble as colliding with itself, we take each
marble out of our list before feeding the quadtree.

Once we have a quadtree, we use `.find` to look for the nearest marble
within two radiuses – `MarbleR*2` – of the current marble. That’s
exactly the one we’re colliding with\! 😄

**Handling collisions with marbles** involves math. The sort of thing
you think you remember from high school, and suddenly realize you don’t
when the time comes to use it.

Code looks like this:

``` javascript
if (candidate) {

    // borrowing @air_hadoken's implementation from here:
    // https://github.com/airhadoken/game_of_circles/blob/master/circles.js#L64
    const cx = candidate.x,
          cy = candidate.y,
          normx = cx - x,
          normy = cy - y,
          dist = (normx ** 2 + normy ** 2),
          c = (_vx * normx + _vy * normy) / dist * 2.3;

    _vx = (_vx - c * normx)/2.3;
    _vy = (_vy - c * normy)/2.3;

    candidate.vx += -_vx;
    candidate.vy += -_vy;
    candidate.x += -_vx;
    candidate.y += -_vy;
}

return {
    x: x + _vx,
    y: y + _vy,
    vx: _vx,
    vy: _vy
}
```

Ok, the `return` statement isn’t about handling collisions. It updates
the current marble.

The rest looks like magic. I implemented it and it still looks like
magic.

You can think of `[normx, normy]` as a vector that points from current
marble to collision candidate. It gives us bounce direction. We use the
[euclidean distance](https://en.wikipedia.org/wiki/Euclidean_distance)
formula to calculate the length of this vector. The distance between the
centers of both marbles.

Then we calculate the [dot
product](https://en.wikipedia.org/wiki/Dot_product) between our marble’s
speed vector and the collision direction vector. And we normalize it by
distance. Multiplying distance by `2` accounts for there being two
marbles in the collision. That extra `.3` made the simulation look
better.

Fiddling and experimentation are your best tools for magic values like
that 😉

Then we use the dot product scalar to adjust the marble’s speed vector.
Dividing by `2` takes into account that half the energy goes to the
other marble. This is true because we assume their masses are equal.

Finally, we update the `candidate` marble and make sure it bounces off
as well. We do it additively because that’s how it happens in real life.

Two marbles traveling towards each other in exactly opposite directions
with exactly the same speed, will stop dead and stay there. As soon as
there’s any misalignment, deflection happens. If one is stationary, it
starts moving. If it’s moving in the same direction, it speeds up… etc.

The end result is [a decent-looking simulation of
billiards](https://swizec.github.io/declarative-canvas-react-konva/).
