---
title: Email Control
description: Documentation about Email Controls inside Newsletter Studio
---
# Email Controls
The actual content inside the email editor is always contained inside a "Email Control", some core controls are:

* Image
* Rich Text
* Button

These controls are displayed in the toolbox on the right hand side inside the email editor.

## Custom Email Control
It's possible to create custom Email Control's and use them for both Campaigns and Transactional emails. Follow this guide to create your own control. Our example is a control that allows for custom HTML to be pasted into the email using a text-area but the possibilities is endless. 

Start by creating a new folder inside the "App_Plugins"-folder of your project, let's call it `NewsletterStudioCustom` (or what ever you would like.)

Inside the folder create another folder called "Models" and add the CS-filers for the Data Model, View Model and Control Type to this folder.

## The Data Model
This model contains the data that your control must store, that could be raw HTML (as for the Rick Text-control) or image-urls, links etc. Each time the control is used inside a email this model will be used to serialize and store the data.

```csharp
public class CustomHtmlEmailControlData : EmailControl
{
    public override string ControlTypeAlias => "customReceipt";

    public string Html { get; set; }

    Padding Padding { get; set; }

}
```

## The View Model
The view model is used when the Razor-engine renders the email, this is what will get passed down to the cshtml-view when the actual email is rendered.

```csharp
public class CustomHtmlEmailControlViewModel : EmailControlViewModelBase
{
    
    public string Html { get; set; }



}
```

## The Control Type
The Control Type is the main definition for the Email Control, this class is responsible for processing data and also holds important meta-data about the Email Control (like alias, icon etc.).

```csharp
public class CustomHtmlEmailControlType : EmailControlTypeBase<CustomHtmlEmailControlData, CustomHtmlEmailControlViewModel>
{
    public override CustomHtmlEmailControlViewModel DoBuildViewModel(CustomHtmlEmailControlData model, EmailControlRenderingData renderingData)
    {
        return new ReceiptEmailControlViewModel()
        {
            Html = model.Html
        };
    }

    public override ReceiptEmailControlViewModel DoUpdateUniqueViewModel(ReceiptEmailControlViewModel model, IRecipientDataModel recipient,
        object transactional)
    {
        // do nothing
        return model;
    }

    public override ReceiptEmailControlData BuildEmptyInstance()
    {
        return new ReceiptEmailControlData()
        {
            Html = "<p>Fallback</p>"
        };
    }

    public override string ViewRender => "/app_plugins/nsext/edit.html";
    public override string ViewEdit => "/app_plugins/nsext/view.html";

    public override string Alias => "customReceipt";
    
    public override string Icon => "icon-home";
    public override string IconSvg => "";
    
}
```

## Configure the Control Type
After these classes has been created you need to tell Newsletter Studio about the EmailControlType, create a Composer and append it to the list of ControlTypes.

```csharp
public class CustomNewsletterStudioComposer : IComposer
{
    public void Compose(Composition composition)
    {
        composition.NewsletterStudio().EmailControlTypes.Append<CustomHtmlEmailControlType>();
    }
}
```

When this is done the Email Control Type should show up in the toolbox, otherwise you might need to go to "Administration -> (Workspace)" and configured the Allowed Email Controls. 

We also need to add translations for our email editor so that we get a nice name in the UI. Create the folder `App_Plugins/NewsletterStudioCustom/Lang` and put a file called `en-US.xml` inside this directory. Use the following content:

```xml
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<language alias="en" culture="en-US">
  <area alias="ns">
    <key alias="control_customHtml">HTML</key>
  </area>
</language>
```

Note the alias, it's a combination of "control_" and the alias of the Email Control Type. This will ensure that english backoffice users get the right text in the UI, of course you can use any language you like, this is standard Umbraco localizations.

Also, we need to create the html-views for the rendering and edit in the backoffice, create these two files as placeholders:

/App_Plugins/NewsletterStudioCustom/CustomHtmlEmailControl/view.html
```html
<div>
    Custom HTML Control, render view
</div>
```

/App_Plugins/NewsletterStudioCustom/CustomHtmlEmailControl/edit.html
```html
<div>
    Custom HTML Control, edit view
</div>
```

And finaly we need to render the cshtml-files for the control, there are several ways to create the cshtml.

**Quick and Dirty**
Just place the cshtml-file called "customhtml.cshtml" inside /App_Plugins/NewsletterStudio/Themes/Default/Views/Controls/customhtml.cshtml. This is not the recommended way as this file might get lost during upgrades.

The recommended way is to create a custom Theme and place the view inside the theme. Create a folder for the theme:
/App_Plugins/NewsletterStudioCustom/NewsletterStudio/Themes/CustomTheme/

Inside this folder, place a file called `themes.json` with the following content:
```javascript
{
  "name": "Custom Theme"
}
```
This will just inherit all settings from the default theme but it will act as a placeholder for our cshtml view.

Create the folder for the view: /App_Plugins/NewsletterStudioCustom/NewsletterStudio/Themes/CustomTheme/Views/Controls/ and create the file `CustomHtml.cshtml` inside this view with the following content:

```html
@inherits NewsletterStudio.Web.Rendering.TemplateBase.RazorTemplateFolderHost<NewsletterStudio.Core.Rendering.ViewModels.ControlWithEmailViewModel>
@using NsTest.App_Plugins.NewsletterStudioCustom.CustomHtmlEmailControl.Models
@{
    var settings = Model.Email.Settings;
}

@if (Model.Model is CustomHtmlEmailControlViewModel customHtml)
{
    @Html.Raw(customHtml.Html)
}
```

NOTE: When the package move into stable release there will be a solution to this so that we can create a cshtml in a folder outside the package but that does not force the creation of a custom theme.

The new control icon should now appear in the toolbox in the backoffice. And you should be able to drag it into the content of the email.

![email-editor--with-toolbox](/media/guides/email-control/toolbox-with-control.png)

## Creating the backoffice experiance




Steps
* Add models (Control Type, Data and ViewModel)
* Add HTML-views for backoffice (edit and view)
* Add custom theme-folder
* Add cshtml-view for alias on Control Type.



TODO:
* [ ] Improve step by step guide.
