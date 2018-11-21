
## 6 Redux Actions

Redux actions are a fancy way of saying *“Yo, a thing happened\!”*.
They’re functions you call to get structured metadata that’s passed
into Redux reducers.

Our particle generator uses 6 actions:

1.  `tickTime` steps our animation to the next frame
2.  `tickerStarted` fires when everything begins
3.  `startParticles` fires when we hold down the mouse
4.  `stopParticles` fires when we release
5.  `updateMousePos` keeps mouse position saved in state
6.  `resizeScreen` saves new screen size so we know where edges lie

Our actions look something like this:

``` javascript
export function updateMousePos(x, y) {
    return {
        type: UPDATE_MOUSE_POS,
        x: x,
        y: y
    };
}
```

A function that accepts params and returns an object with a type and
meta data. Technically this is an action generator and the object is an
action, but that distinction has long since been lost in the community.

Actions *must* have a `type`. Reducers use the type to decide what to
do. The rest is optional.

You can see [all the actions on
GitHub](https://github.com/Swizec/react-particles-experiment/blob/master/src/actions/index.js).

I find this to be the least elegant part of Redux. Makes sense in large
applications, but way too convoluted for small apps. Simpler
alternatives exist like doing it yourself with React Context.
