
## Animated alphabet

Our goal is to render a random subset of the alphabet. Every time the
set updates, old letters transition out, new letters transition in, and
updated letters transition into a new position.

We need two components:

  - `Alphabet`, which creates random lists of letters every 1.5 seconds,
    then maps through them to render `Letter` components
  - `Letter`, which renders an SVG text element and takes care of its
    own enter/update/exit transitions

You can see the full code on GitHub
[here](https://github.com/Swizec/react-d3-enter-exit-transitions/blob/master/src/components/Alphabet.jsx).
