---
layout: default
title: Includes
---

Although we encourage keeping your code contained within the Framework guidelines, We've made it easy to reference external files, incorporate "raw" Javascript, and drop down to the Famous Engine whenever necessary. 

## Loading external CSS and JS

To include external CSS or JavaScript with your module, first place the files into your project directory, for example:

    ├── ...
    └── jane-doe/
        └── hello-famous/
            ├── hello-famous.js
            ├── foo.css
            └── bar.js

Then, indicate that you want to load these files by using the `config` method, which can be chained to your main component definition:

    FamousFramework.component('jane-doe:hello-famous', {
        // etc
    })
    .config({
        includes: [
            'foo.css',
            'bar.js'
        ]
    });

The assets will be added to the document `<head>` in the order they were found in the `includes` array, and your component will only be initialized after they are loaded.

## Using "raw" JavaScript

Although we recommend sticking to the BEST pattern as closely as possible, sometimes you just need a way to write some raw JavaScript code without the Famous Framework getting in the way. Fortunately, you can do this without needing any additional setup. Just include the code anywhere outside of the `FamousFramework.component` invocation.

    var hiddenState = 1.0;
    function myHelperFunction() {
        return hiddenState * Math.random();
    }
    FamousFramework.component('jane-doe:hello-best', {
        // etc
    });

Note that your entrypoint file, e.g. `jane-doe/hello-famous/hello-famous.js` should have at least one call to `FamousFramework.component` to get the benefits of Famous cloud services.

## Using "raw" Famous

Some developers need a way to drop down to the low-level Famous Engine in order to get the job done. Luckily, this is just as easy as using [raw JavaScript code](raw-code.md). We make the full Famous library available exposed as a global `Famous` object, which you can tap into to squeeze the most out of the engine.

    var context = Famous.createContext();
    var root = context.addChild();
    var el = new Famous.domRenderables.DOMElement(root);
    el.setProperty('background', 'yellow');
    FamousFramework.component('jane-doe:hello-best', {
        // etc
    });

Note that your entrypoint file, e.g. `jane-doe/hello-best/hello-best.js` should have at least one call to `FamousFramework.component` to get the benefits of Famous cloud services.