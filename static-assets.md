---
layout: default
title: Static Assets
---

# Using static assets

To use static assets (such as images) with your component, simply include them anywhere within your project folder. For example:

    ├── ...
    └── jane-doe/
        └── hello-famous/
            ├── hello-famous.js
            └── my-image.jpg

Then, within your `hello-famous.js` entrypoint file, you can refer to that asset using special syntax for asset interpolation, `{% raw %}{{BASE_URL}}{% raw %}`:

    FamousFramework.component('jane-doe:hello-famous', {
        tree:`<node id="imageNode">
            <img src="{% raw %}{{BASE_URL}}{% raw %}my-image.png">
        </node>`
    });


Refer to [famous-tests:static-assets](https://github.com/Famous/framework/blob/develop/lib/core-components/famous-tests/static-assets/static-assets.js) for a working example of this concept.
