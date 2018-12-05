
# Using a React alternative like Preact or Inferno

We’ve been using React so far, and that’s worked great. React is fast,
easy to use, and not too hard to understand. However, two alternative
frameworks promise the ease of React, but faster with a smaller
footprint.

[**Preact**](https://github.com/developit/preact) looks just like React
when you’re using it, but the smaller code footprint means your apps
open faster. Performing fewer sanity checks at runtime makes it faster
to run too. Async rendering can make your apps feel even faster.

[**Inferno**](https://github.com/infernojs/inferno) also promises to
look just like React when you’re using it, but its sole purpose in life
is to be faster. According to its maintainers, converting to Inferno can
improve performance of your real-world app by up to 110%.

Both Preact and Inferno have a `-compat` project that lets you convert
existing React projects without any code modifications. While React has
gotten much faster in recent years, so have Preact and Inferno. You
should give them a try.

I know Uber uses Preact for their Uber lite mobile
app.
