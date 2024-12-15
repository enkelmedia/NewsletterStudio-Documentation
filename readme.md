# Newsletter Studio Documentation

This repository contains markdown files for the Newsletter Studio-documentation, these files is used for the official documentation hosted on [newsletterstudio.org](https://www.newsletterstudio.org).

We welcome contributions.

## Markdown Macros
The documentation system supports a couple of custom "Markdown Macros", that is special tags that can be used to render content and elements.

### Contrib-code
This macro supports downloading code from the [NewsletterStudioContrib](https://github.com/enkelmedia/NewsletterStudioContrib)-project on Github.

The URL should start with the current version number and then be relative.

Ex:

{% contrib file="V14/Extensions-Demos/Demo.Web/Extensions/EmailEditorControl/HelloEmailControlType.cs" %}
{% endcontrib %}

### Hints

Examples:

```
{% hint style="info" %}
This is some important information
* Foo The actual content inside the email editor is always contained inside an The actual content inside the email editor is always contained inside an
* Bar The actual content inside the email editor is always contained inside an The actual content inside the email editor is always contained inside an
* 123 The actual content inside the email editor is always contained inside an The actual content inside the email editor is always contained inside an
{% endhint %}
```
or 

```
{% hint style="warning" %}
**Do not** try this at home
{% endhint %}
```

