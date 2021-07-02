---
title: Email Editor
description: Documentation about The Email Editor inside Newsletter Studio
---
# Email Editor

The buildt in editor makes the creative process of designing responsive emails for desktop, mobile and tablets a breeze.

![email-editor--edit](/media/email-editor--edit.png)

The main window in the editor contains a design-surface to the left and a toolbox to the right. To add new items to the design surface, just drag and drop from the toolbox. Clicking one of the items in the design surface will activate the edit-mode for this row, or control.



## The Layout structure

The email consists of multiple stacked rows, each row has a number of columns and each column contains controls. It's the [Email Controls](../develop/email-control.md) that contains the actual content of the email.

### Rows

The rows are stacked on top of each other and supports some settings in the toolbox, for example:

* **Full width** will make the row span the full width of the screen, by default the width of the content is 600px.
* **Stack on small screens** When checked and if the row has multiple columns the content will be stacked on smaller screens.
* **Background** Choose a background color for the row
* **Background Image** Choose a background image for the row

To add a new Row to the layout, just scroll to the bottom and click on the "Add row"-button.

### Email Controls

Email controls contains the actual content in the email, like text images and buttons. They are found when clicking "Insert" in the Toolbox. To add a control to a Column in the design just drag and drop it. 

Each control has different settings but most of them has settings for

* **Margins** Use this to control the whitespace around the control, must controls default to 10px margin in all directions.
* **Alignment** Change the alignment between left, middle or right.
* **Color** Text and button controls will have color-settings



#### Custom Email Controls

It's possible to create custom controls, please follow our [guide of custom email controls](../develop/email-control.md) for more details.


