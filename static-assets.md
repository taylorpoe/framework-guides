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

Then, within your `hello-famous.js` entrypoint file, you can refer to that asset using special syntax for asset interpolation, `{% raw %}{{@}}{% endraw %}`:

{% raw %}
    FamousFramework.component('jane-doe:hello-famous', 'HEAD', {
        tree: `<ui-element><img src="{{@my-image.jpg}}"></ui-element>`
    });
{% endraw %}

Alternatively, if you need to do dynamic asset interpolation or need to refer to assets that are hosted inside another module, the `{{@CDN_PATH}}` syntax should be used:

```
{% raw %}
      behaviors: {
          '#ui-element-1': {
              content: function(imagePath) {
                  // Alternative syntax for inferring asset path name.
                  // {{@CDN_PATH}} is a keyword that will be evaluated during compilation
                  // to match the CDN location where this component is hosted.
                  return '<img src="{{@CDN_PATH}}' + imagePath + '">';
              }
          },
          '#ui-element-2': {
              content: function(otherImagePath) {
                  // {{@CDN_PATH|username:component}} is a keyword that will be evaluated during compilation
                  // to match the CDN location where `username:component` is hosted.
                  return '<img src="{{@CDN_PATH|foo:bar}}' + otherImagePath + '">';
              }
          }
      }
{% endraw %}
```

Refer to [famous-tests:static-assets](https://github.com/Famous/framework/blob/develop/lib/core-components/famous-tests/static-assets/static-assets.js) for a working example of this concept.