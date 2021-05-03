---
title: Merge Field Providers
description: Documentation about Merge Field Providers inside Newsletter Studio
---
# Merge Field Providers
The email editor supports merge fields, that is placeholders for dynamic content that will be replaced during the rendering. This makes it very easy for editors to insert dynamic content inside the editor.

Some of the built in merge fields are:
* {{email}} - Replaced by the recipients email
* {{name}} - Replaces by the recipients name

Each "Custom Field" on a recipient is also represented as a merge field in the editor.

## Extending Merge Fields
It's possible to add merge fields to the editor, this is useful if you create a custom integration and would like to append more dynamic fields that the editor can choose from inteeed of having to remember the exact name, this is done using a "Merge Field Provider.


TODO:
* [ ] Provide code-example and guide
* [ ] Add screenshot of Merge Field-picker in RTE.
