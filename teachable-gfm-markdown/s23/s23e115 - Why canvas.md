
## Why Canvas

Unlike SVG, HTML5 Canvas lets you draw rasterized images. This means
you’re no longer operating at the level of shapes because you’re
working with pixels on the screen.

With SVG and other vector formats, you tell the browser *what* you want
to render. With Canvas and other raster formats, you tell the browser
*how* you want to render. The browser doesn’t know what you’re doing; it
gets a field of pixel colors and renders them as an image.

That’s much faster for computers to handle. In some cases browsers can
even use hardware acceleration – the GPU – to render HTML5 Canvas
elements. With a bit of care, you can do almost anything you want, even
on a mobile phone.

Phones these days have amazing GPUs and kind of terrible CPUs in
comparison. The CPU burns more battery, works slower, warms up your
phone more, etc.

If SVG wasn’t so easy to use, I’d almost suggest going straight to
Canvas for any sort of complex animation. Mobile traffic is, what, 60%
to 70% of web traffic these days?
