
# Enter/update/exit animation

Now that you know how to use transitions, it’s time to take it up a
notch. Enter/exit animations.

Enter/exit animations are the most common use of transitions. They’re
what happens when a new element enters or exits the picture. For
example: in visualizations like this famous [Nuclear Detonation
Timeline](https://www.youtube.com/watch?v=LLCF7vPanrY) by Isao
Hashimoto. Each new Boom\! flashes to look like an explosion.

I don’t know how Hashimoto did it, but with React & D3, you’d do it with
enter/exit transitions.

Another favorite of mine is this [animated
alphabet](https://bl.ocks.org/mbostock/3808234) example by Mike Bostock,
the creator of D3, that showcases enter/update/exit transitions.

That’s what we’re going to build: An animated alphabet. New letters fall
down and are green, updated letters move right or left, deleted letters
are red and fall down.

You can play with a more advanced version
[here](http://swizec.github.io/react-d3-enter-exit-transitions/). Same
principle as the alphabet, but it animates what you type.

![Typing animation
screenshot](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/typing-screenshot.png)

We’re building the alphabet version because the [string diffing
algorithm](https://swizec.com/blog/animated-string-diffing-with-react-and-d3/swizec/6952)
is a pain to explain. I learned that the hard way when giving workshops
on React and D3…

![String diffing algorithm
sketch](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/string-diffing.jpg)

See?

Easy on paper, but the code is long and weird. That, or I’m bad at
implementing it. Either way, it’s too tangential to explain here. You
can [read the article about
it](https://swizec.com/blog/animated-string-diffing-with-react-and-d3/swizec/6952).
