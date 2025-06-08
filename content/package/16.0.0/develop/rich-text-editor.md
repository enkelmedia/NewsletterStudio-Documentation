---
title: Configure the Rich Text Editor
description: Here you can read about how to configure the Rich Text Editor, Tiptap, when used in Newsletter Studio
---
# Customize the Rich Text Editor
Our email editor comes with a powerful Rich Text Editor powered by Tiptap. To give you full control over the editing experience, we've made it possible to configure the editor to suit your project’s specific requirements.

As a developer, you can define which extensions are enabled, adjust settings, and customize the toolbar to match your content model or editorial workflow. Whether you're aiming for a simplified interface or want to expose advanced formatting options, the configuration is flexible and fully under your control.

## Umbraco and Tiptap
Umbraco ships with [Tiptap](https://tiptap.dev/) as it's core Rich Text Editor, to follow this guide we expect that you have the basic knowledge needed to create [Backoffice Extensions](https://docs.umbraco.com/umbraco-cms/customizing/overview) and that you understand how [Tiptap is integrated into Umbraco](https://docs.umbraco.com/umbraco-cms/fundamentals/backoffice/property-editors/built-in-umbraco-property-editors/rich-text-editor).

A fewer important points to keep in mind:

* Tiptap calls it self a Rich Text Editor "Toolkit".
* Every piece of functionality in Tiptap is provided by a [Tiptap extension](https://tiptap.dev/docs/editor/extensions/overview), not to confuse with [Umbraco Extension Types](https://docs.umbraco.com/umbraco-cms/customizing/extending-overview/extension-types).
* Umbraco provides two main Extension Types for the Rich Text Editor
  * `tiptapToolbarExtension` used to add toolbar items (buttons and other elements)
  * `tiptapExtension` used to load Tiptap Extensions that's not shipped with Umbraco by default.

## Our extension point
Newsletter Studio reuses the Tiptap-implementation from Umbraco, which means that we have to provide a Umbraco-specific configuration. This configuration is the same configuration that is needed when the Tiptap used in a `Data Type`.

![Rich Text Editor configuration in Umbraco](/media/tiptap/rich-text-editor--configuration.png)

Newsletter Studio comes with a [default configuration](#default-configuration), but we provide a Umbraco Extension Type called `nsTiptapConfiguration` that allows you to customize or replace the default configuration for Tiptap.

The extension requires a implementation of the interface `NsTiptapConfigurationSource`, for example like this:

{% contrib file="V16/Extensions-Demos/Demo.Web/Client/src/tiptap-configuration/tiptap-configuration.ts" %}
{% endcontrib %}

We also need to register our extension with Umbraco:

{% contrib file="V16/Extensions-Demos/Demo.Web/Client/src/tiptap-configuration/manifest.ts" %}
{% endcontrib %}

## Default configuration
The default configuration looks like this:

```javascript
[
  {
    alias: "overlaySize",
    value: "medium"
  },
  {
    alias: "height",
    value: 900
  },
  {
    alias: "maxImageSize",
    value: 500
  },
  {
    alias: "toolbar",
    value : [
      [
        [
          "Umb.Tiptap.Toolbar.SourceEditor",
        ],
        [
          "Ns.Tiptap.Toolbar.StyleMenu",
        ],
        [
          "Umb.Tiptap.Toolbar.Bold",
          "Umb.Tiptap.Toolbar.Italic",
          "Umb.Tiptap.Toolbar.Underline",
          "Umb.Tiptap.Toolbar.Strike"
        ],
        [
          "Umb.Tiptap.Toolbar.TextAlignLeft",
          "Umb.Tiptap.Toolbar.TextAlignCenter",
          "Umb.Tiptap.Toolbar.TextAlignRight"
        ],
        [
          "Umb.Tiptap.Toolbar.BulletList",
          "Umb.Tiptap.Toolbar.OrderedList"
        ],
        [
          "Umb.Tiptap.Toolbar.Link",
          "Umb.Tiptap.Toolbar.Unlink"
        ],
        [
          "Ns.TipTap.Toolbar.ColorPicker",
          "Ns.TipTap.Toolbar.MergeFields"
        ]
      ]
    ]
  },
  {
    alias: "extensions",
    value: [
      "Umb.Tiptap.Embed",
      "Umb.Tiptap.Figure",
      "Umb.Tiptap.Image",
      "Umb.Tiptap.Link",
      "Umb.Tiptap.MediaUpload",
      "Umb.Tiptap.RichTextEssentials",
      "Umb.Tiptap.Subscript",
      "Umb.Tiptap.Superscript",
      "Umb.Tiptap.Table",
      "Umb.Tiptap.TextAlign",
      "Umb.Tiptap.TextDirection",
      "Umb.Tiptap.Underline"
    ]
  },
  {
    "alias": "stylesheets",
    "value": [
        "/css/Newsletter-Studio.css"
    ]
  }
];
```