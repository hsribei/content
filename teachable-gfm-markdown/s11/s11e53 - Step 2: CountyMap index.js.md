
## Step 2: CountyMap/index.js

We make `index.js` for just reason: to make imports and debugging
easier. I learned this lesson the hard way so you don’t have to.

    // src/components/CountyMap/index.js
    
    export { default } from './CountyMap';

Re-export the default import from `./CountyMap.js`. That’s it.

This allows us to import `CountyMap` from the directory without knowing
about internal file structure. We *could* put all the code in this
`index.js` file, but that makes debugging harder. You’ll have plenty of
index.js files in your project.

Having a lot of code in `<directory>/index.js` is a common pattern in
new projects. But when you’re looking at a stack trace, source files in
the browser, or filenames in your editor, you’ll wish every component
lived in a file of the same name.
