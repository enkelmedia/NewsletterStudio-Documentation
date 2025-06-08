---
title: Signup form
description: Documentation about signup forms inside Newsletter Studio
---
# Signup form
During the first boot after installing the package we will copy a partial view called `NewsletterStudioSignup.cshtml` into the views-folder of the Umbraco-project.

This view can be used as a starting point for a simple signup form, you could use it in a block or render it directly on a page using something like this:

```html
<partial name="NewsletterStudioSignup" />
```

This simple demo will just add the recipient to the first mailing list it can find. While this works just fine, the file is intended more as a starting point.

When you create your own signup form it's very likely that you need to use the INewsletterStudioService to [add a recipient programmatically](../develop/front-end-api.md)
