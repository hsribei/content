
# Using transitions for simple animation

Game loops are great for fine-grained control. And when all you need is
a little flourish on user action, that’s where transitions shine.

No details, just keyframes.

Transitions let you animate SVG elements by saying *“I want this
property to change to this new value and take this long to do so”*.

Start-end keyframes are the simplest. You define the starting position.
Start a transition. Define the end position. D3 figures out the rest.
Everything from calculating the perfect rate of change to match your
start and end values and your duration, to *what* to change and handling
dropped frames.

Quite magical.

You can also sequence transitions to create complex keyframe-based
animation. Each new transition definition is like a new keyframe. D3
figures out the rest.

Better yet, you can use easing functions to make your animation look
more natural. Make rate of change follow a mathematical curve to create
smooth natural movement.

You can read more about the *why* of easing functions in Disney’s [12
Basic Principles of
Animation](https://en.wikipedia.org/wiki/12_basic_principles_of_animation).
Bottom line is that it makes your animation feel natural.

*How* they work is hard to explain. You can grok part of it in my
[Custom transition
tweens](https://swizec.com/blog/silky-smooth-piechart-transitions-react-d3js/swizec/8258)
article.

But don’t worry about it. All you have to know is that many easing
functions exist. [easings.net](https://easings.net/) lists the common
ones. D3 implements everything on that list.

![Common easing
functions](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/2018/easings.net.png)

Let’s try an example: A swipe transition.
