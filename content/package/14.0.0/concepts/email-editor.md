---
title: Email Editor
description: Documentation about The Email Editor inside Newsletter Studio
---
# Email Editor

The built-in editor makes the creative process of designing responsive emails for desktop, mobile, and tablets a breeze.

![email-editor--edit](/media/email-editor--edit.png)

The main window in the editor contains a design surface to the left and a toolbox to the right. To add new items to the design surface, just drag and drop from the toolbox. Clicking one of the items in the design surface will activate the edit mode for this row or control.


## The Layout structure

The email consists of multiple stacked rows, each row has a number of columns and each column contains controls. It's the [Email Controls](../develop/email-control.md) that contains the actual content of the email.

### Rows

The rows are stacked on top of each other and support some settings in the toolbox, for example:

* **Full width** will make the row span the full width of the screen, by default the width of the content is 600px.
* **Stack on small screens** When checked and if the row has multiple columns the content will be stacked on smaller screens.
* **Background** Choose a background color for the row
* **Background Image** Choose a background image for the row

To add a new Row to the layout, just scroll to the bottom and click on the "Add row"-button.

### Email Controls

Email controls contain the actual content in the email, like text images and buttons. They are found when clicking "Insert" in the Toolbox. To add a control to a Column in the design just drag and drop it. 

Each control has different settings but most of them have settings for

* **Margins** Use this to control the whitespace around the control, most controls default to 10px margin in all directions.
* **Alignment** Change the alignment between left, middle, or right.
* **Color** Text and button controls will have color-settings

#### Custom Email Controls

It's possible to create custom controls, please follow our [guide of custom email controls](../develop/email-control.md) for more details.

## Merge Fields
It's easy to insert dynamic content into the messages, use the "Field's"-dropdown in the text editor to get a list of available fields. These might differ depending on if you're sending a Campaign or Transactional Email and also depending on what type of [Merge Field Providers](../develop/merge-field-providers.md) are activated.

To use a Merge Field inside the email just put its placeholder inside double brackets, e.g: `{{email}}`, `{{firstname}}` or `{{lastname}}`. 

### Fallbacks
It's also possible to provide a fallback if the merge field is not found, `{{age:"No age"}}` in this case the text "No age" will be written if the merge field `age` is not found.

## Fonts
The list of fonts is populated with the most common standard fonts and a list of web fonts from Google. Not all email clients support web fonts, so it's recommended to use any of the standard fonts.

### Adding fonts
The font-list is populated with some common fonts but it's also possible to extend this list. See the [Themes](../concepts/themes.md)-section for more details.



