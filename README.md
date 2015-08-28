# What is this?

This is an example of promise-based script loading that will work in older browsers
as well as newer.

The scenario is that we may have to load polyfill scripts, and if so, these need to
be loaded into the page before we load our main bundle of dependencies, as this
relies on the functionality that is being polyfilled.

# Features

Features include:

* inlining of the minified version of the [promise-polyfill](https://github.com/taylorhakes/promise-polyfill), which adds around 2.5KB of weight to the page, but saves browsers without `window.Promise` having to make another (blocking) request

* a script loading function that returns a promise for the script being loaded, resolved by the `load` event.

* a test to [cut the mustard](http://responsivenews.co.uk/post/18948466399/cutting-the-mustard), which allows us to determine what is the cut-off point, below which we aim for a less-augmented experience (i.e. we don't bother to polyfill the browser's lack of feature support)

* polyfill examples, using JavaScript stubs

* asynchronous loading of polyfills (though if synchronous polyfill loading is needed, we can chain the polyfill promises)

* a bundle of dependencies, which are only loaded once the polyfills are in place (using `Promise.all`)

* namespacing of our code to prevent leakage of variables / variable conflicts into the global space

* some `console.logs` showing the time that the scripts are loaded, using `window.performance.now`

Required reading is the [HTML5Rocks article by Jake Archibald on script loading](http://www.html5rocks.com/en/tutorials/speed/script-loading/). Ironically, Jake is the author of the [es6-promise polyfill](https://github.com/jakearchibald/es6-promise), which we forego in favour of the [promise-polyfill](https://github.com/taylorhakes/promise-polyfill), simply because the latter clocks in at around 2.5KB, compared to 17KB for the former

For our purposes, we only need basic `window.Promise` support, and can live without the features of the more complete `es6-promise` polyfill.

What we're trying to accomplish here is script-loading that will work across a wide range of browsers, but which is also fast and relatively lightweight.

Because we may need certain polyfills, we have to load these before our main bundle of dependencies &#8212; `window.Promise.all` comes in very handy here.
