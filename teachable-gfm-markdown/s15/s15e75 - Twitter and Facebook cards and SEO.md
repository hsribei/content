
## Twitter and Facebook cards and SEO

How your visualization looks on social media matters more than you’d
think. Does it have a nice picture, a great description, and a title, or
does it look like a random URL? Those things matter.

And they’re easy to set up. No excuse.

We’re going to poke around `public/index.html` for the first time. Add
titles, Twitter cards, Facebook Open Graph things, and so on.

``` html
<!-- public/index.html -->
<head>
    <!-- //... -->
    // Insert the line(s) between here...
    <title>How much does an H1B in tech pay?</title>

    <link rel="canonical"
          href="https://swizec.github.io/react-d3js-step-by-step/" />
    // ...and here.
</head>
<body>
    <!-- //... -->
    <div id="root">
        // Insert the line(s) between here...
         <h2>The average H1B in tech pays $86,164/year</h2>

     <p class="lead">
             Since 2012 the US tech industry has sponsored 176,075
             H1B work visas. Most of them paid <b>$60,660 to $111,668</b>
             per year (1 standard deviation). <span>The best city for
             an H1B is <b>Kirkland, WA</b> with an average individual
             salary <b>$39,465 above local household median</b>.
             Median household salary is a good proxy for cost of
             living in an area.</span>
         </p>
        // ...and here.
    </div>
</body>
```

We add a `<title>` and a `canonical` URL. Titles configure what shows up
in browser tabs, and the canonical URL is there to tell search engines
that this is the main and most important URL for this piece of content.
This is especially important for when people copy-paste your stuff and
put it on other websites.

In the body root tag, we add some copy-pasted text from our dataviz.
You’ll recognize the default title and description.

As soon as React loads, these get overwritten with our preloader, but
it’s good to have them here for any search engines that aren’t running
JavaScript yet. I think both Google and Bing are capable of running our
React app and getting text from there, but you never know.

To make social media embeds look great, we’ll use [Twitter
card](https://dev.twitter.com/cards/types/summary-large-image) and
[Facebook
OpenGraph](https://developers.facebook.com/docs/sharing/webmasters) meta
tags. I think most other websites just rely on these since most people
use them. They go in the `<head>` of our HTML.

``` javascript
<!-- public/index.html -->
<head>
    <meta property="og:locale" content="en_US" />
    <meta property="og:type" content="article" />
    <meta property="og:title"
          content="The average H1B in tech pays $86,164/year" />
    <meta property="og:description"
          content="Since 2012 the US tech industry has sponsored
176,075 H1B work visas. With an average individual salary
up to $39,465 above median household income." />
    <meta property="og:url"
          content="https://swizec.github.io/react-d3js-step-by-step" />
    <meta property="og:site_name" content="A geek with a hat" />
    <meta property="article:publisher"
          content="https://facebook.com/swizecpage" />
    <meta property="fb:admins" content="624156314" />
    <meta property="og:image"
          content="https://swizec.github.io/react-d3js-step-by-step/thumbnail.png" />

    <meta name="twitter:card" content="summary_large_image" />
    <meta name="twitter:description"
          content="Since 2012 the US tech industry has sponsored
176,075 H1B work visas. With an average individual salary
up to $39,465 above median household income." />
    <meta name="twitter:title"
          content="The average H1B in tech pays $86,164/year" />
    <meta name="twitter:site" content="@swizec" />
    <meta name="twitter:image"
          content="https://swizec.github.io/react-d3js-step-by-step/thumbnail.png" />
    <meta name="twitter:creator" content="@swizec" />
</head>
```

Much of this code is repetitive. Both Twitter and Facebook want the same
info, but they’re stubborn and won’t read each other’s formats. You can
copy all of this, but make sure to change `og:url`, `og:site_name`,
`article:publisher`, `fb:admins`, `og:image`, `twitter:site`,
`twitter:image`, and `twitter:creator`. They’re specific to you.

The URLs you should change to the `homepage` URL you used above. The
rest you should change to use your name and Twitter/Facebook handles.
I’m not sure *why* it matters, but I’m sure it does.

An important one is `fb:admin`. It enables admin features on your site
if you add their social plugins. If you don’t, it probably doesn’t
matter.

You’re also going to need a thumbnail image. I made mine by taking a
screenshot of the final visualization, then I put it in
`public/thumbnail.png`.

Now when somebody shares your dataviz on Twitter or Facebook, it’s going
to look something like this:

![Dataviz Twitter
card](https://raw.githubusercontent.com/Swizec/react-d3js-es6-ebook/2018-version/manuscript/resources/images/es6v2/twitter-card.png)
