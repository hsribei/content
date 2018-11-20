
### 2\) Scales

Scales are D3’s most versatile concept. They help you translate between
two different spaces. Like, mathematical spaces.

They’re like the mathematical functions you learned about in school. A
domain maps to a range using some sort of formula.

![A basic
function](https://upload.wikimedia.org/wikipedia/commons/thumb/d/df/Function_color_example_3.svg/440px-Function_color_example_3.svg.png)

Colored shapes in the domain map to colors in the range. No formula for
this one. That makes it an ordinal scale.

``` javascript
let shapes = d3.scaleOrdinal()
    .domain(['red', 'orange', ...)
    .range(['red', 'orange', ...)
```

[Play with scales on CodeSandbox](https://codesandbox.io/s/r0rw72z75o)

Once you have this scale, you can use it to translate from shapes to
colors. `shapes('red triangle')` returns `'red'` for example.

Many different types of scales exist. Linear, logarithmic, quantize,
etc. Any basic transformation you can think of exists. The rest you can
create by writing custom scales.

You’re most often going to use scales to turn your data values into
coordinates. But other use-cases exist.
