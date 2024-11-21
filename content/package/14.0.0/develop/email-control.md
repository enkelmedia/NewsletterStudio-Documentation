---
title: Email Control
description: Documentation about Email Controls inside Newsletter Studio
---
# Email Controls
The actual content inside the email editor is always contained inside an `Email Control`, some core controls are:

* Image
* Rich Text
* Button
* Macro

These controls are displayed in the toolbox on the right-hand side inside the email editor.

## Custom Email Control
It's possible to create custom Email Controls and use them for both Campaigns and Transactional emails. Follow this guide to create your own control. Our example is a control that allows for custom Hello-message to be pasted into the email using a text area, but the possibilities are endless. 

This guide will build a custom control, the full source can be found in the [Newsletter Studio Contrib-project](https://github.com/enkelmedia/NewsletterStudioContrib/tree/master/Newsletter%20Studio%20V14/Extensions-Demos) on Github.

Start by creating a this folder inside your Umbraco-project: `Extensions/EmailEditorControl`, inside this folder we need to create some C#-files for our custom control. 

## The Data Model
This model contains the data that your control must store, which could be raw HTML (as for the Rick Text-control) or image-urls, links, etc. Each time the control is used inside an email this model will be used to serialize and store the data.

Create a file called `HelloEmailControlData.cs` with the following content:

```csharp
{% contrib file="V14/Extensions-Demos/Demo.Web/Extensions/EmailEditorControl/HelloEmailControlData.cs" %}
{% endcontrib %}
```

## The View Model
The view model is used when the Razor-engine renders the email, this is what will get passed down to the cshtml-view when the actual email is rendered.

In the same folder, create a file called `HelloEmailControlViewModel.cs`:

```csharp
{% contrib file="V14/Extensions-Demos/Demo.Web/Extensions/EmailEditorControl/HelloEmailControlViewModel.cs" %}
{% endcontrib %}
```

## The Control Type
The Control Type is the main definition for the Email Control, this class is responsible for processing data and also holds important meta-data about the Email Control (like alias, icon, etc.).

Create a file called `HelloEmailControlType.cs`:

```csharp
{% contrib file="V14/Extensions-Demos/Demo.Web/Extensions/EmailEditorControl/HelloEmailControlType.cs" %}
{% endcontrib %}
```

## Configure the Control Type
After these classes have been created you need to tell Newsletter Studio about the EmailControlType, create a Composer and append it to the list of ControlTypes.

```csharp
{% contrib file="V14/Extensions-Demos/Demo.Web/Extensions/EmailEditorControl/HelloEmailControlComposer.cs" %}
{% endcontrib %}
```

When this is done the Email Control Type should show up in the email editor toolbox, otherwise, you might need to go to "Administration -> (Workspace)" and configure the "Allowed Email Controls".

## Creating the backoffice experience

At this point you need a setup for backoffice extensions. The official [umbraco documentation](https://docs.umbraco.com/umbraco-cms/14.latest/customizing/development-flow) has a guide on how to setup the tools needed. There are plenty of ways to setup a frontend development environment for Umbraco, in this guide we'll use a fairly common approach that includes using TypeScript and Lit. You can have a look at the `Client`-folder of our [the NewsletterStudioContrib](https://github.com/enkelmedia/NewsletterStudioContrib/tree/master/Newsletter%20Studio%20V14/Extensions-Demos/Demo.Web/Client)-project for inspiration.

From now on we'll assume that you already have the frontend environment setup in the `/Client` folder of the web-project and we'll also assume that you have basic knowledge about extending the backoffice, like registering extensions etc.

### Localization

First we need to ensure that we have localization entries for our new control, add a `localization`-extension and register the manifest into the backoffice.

This will give a friendly name to our newly created control:

```typescript
{% contrib file="V14/Extensions-Demos/Demo.Web/Client/src/localization/en-us.ts" %}
{% endcontrib %}
```

Note the alias, it's a combination of "ns_control_" and the alias of the Email Control Type. This will ensure that English backoffice users get the right text in the UI, of course, you can use any language you like, this is standard Umbraco localizations.

### Edit view

If your workspace configuration allows for it, the new control icon should now appear in the email editor toolbox. You should be able to drag it into the content of the email.

![email-editor--with-toolbox](/media/guides/email-control/toolbox-with-control-vnext.png)

We now need to add two new extension to handle the visual representation inside the email editor, these extensions are of the types:

* **nsEmailControlEditUi**, will be the component to render when showing the edit-view for a selected control.
* **nsEmailControlDisplayUi**, will be the content rendered inside the email editor during editing.

First, we need to declare a typescript model for the data that our control needs to work with, let's create a file called `demo-email-editor-control-hello.models.ts` with the following content:

```typescript
{% contrib file="V14/Extensions-Demos/Demo.Web/Client/src/editor-controls/hello/demo-email-editor-control-hello.models.ts" %}
{% endcontrib %}
```

Let's start with the edit view, in your project add a file called `demo-email-editor-control-hello-edit.element.ts` with this content:

```typescript
{% contrib file="V14/Extensions-Demos/Demo.Web/Client/src/editor-controls/hello/demo-email-editor-control-hello-edit.element.ts" %}
{% endcontrib %}
```

Register this component as a extension of type `nsEmailControlEditUi`:

```typescript
import { ManifestEmailControlEditUi } from '@newsletterstudio/umbraco/extensibility'

const editUi : ManifestEmailControlEditUi = {
    type: 'nsEmailControlEditUi',
    alias : 'nsEmailControlEditUiHello',
    name : 'NS Email Control Edit Ui Hello',
    element : ()=> import('./demo-email-editor-control-hello-edit.element.js'),
    meta : {
      controlTypeAlias : 'hello'
    }
  }
```

Notice here that we're extending `NsEmailEditorControlEditUiBase` which is a base class for edit views. It provides access to some methods to work with the control data, for example `updateModel()`, that takes a partial instance of the data class and updates the model that will be stored.

In our example the data looks something like this

```javascript
{
  "controlTypeAlias": "hello",
  "text": "....",
  "rowAndColumnInfo": {
    "columns": 4,
    "stackOnSmallScreen": false
  },
  "meta": {
      "addedBy" : "customCSharpCode"
  }
}
```

The "meta"-property can be used to pass meta-data from the server to the email control and is not stored in the database. See the C#-example above where we're adding the `addedBy` property to the meta-object.

### Display view

Next, we need to make sure that the preview is rendering the content from the textarea, create a file called `demo-email-editor-control-hello-display.element.ts` that looks like this:

```typescript
{% contrib file="V14/Extensions-Demos/Demo.Web/Client/src/editor-controls/hello/demo-email-editor-control-hello-display.element.ts" %}
{% endcontrib %}
```

We also need to register this extension

```typescript
import { ManifestEmailControlDisplayUi } from '@newsletterstudio/umbraco/extensibility'

const displayUi : ManifestEmailControlDisplayUi = {
    type: 'nsEmailControlDisplayUi',
    alias : 'nsEmailControlDisplayUiHello',
    name : 'NS Email Control Display Ui Hello',
    element : ()=> import('./demo-email-editor-control-hello-display.element.js'),
    meta : {
        controlTypeAlias : 'hello'
    }
}
```

With all this in place you, should have a working Email Control for working in the email editor.

## Rendering the email content

Finally, we need to create a Razor cshtml-file for that will be used to render the HTML into the emails.

To provide a default rendering we recommend that you put the view in the `App_Plugins/NewsletterStudioExtensions/Views/Controls`-folder. However if you have a [Custom theme](../concepts/themes.md), you could also place the view inside your theme.

In the `Controls`-folder, create a file with the same name as your email control type alias, in our case `hello.cshtml`.

```html
{% contrib file="V14/Extensions-Demos/Demo.Web/App_Plugins/newsletterStudioExtensions/views/controls/hello.cshtml" %}
{% endcontrib %}
```
