---
title: Email Control
description: Documentation about Email Controls inside Newsletter Studio
---
# Email Controls
The actual content inside the email editor is always contained inside a "Email Control", some core controls are:

* Image
* Rich Text
* Button

## Custom Email Control
It's possible to create a custom Email Control to be used inside your implementation. 

## The Data Model

```csharp
public class ReceiptEmailControlData : EmailControl
{
    public override string ControlTypeAlias => "customReceipt";

    public string Html { get; set; }

    Padding Padding { get; set; }

}
```

## The View Model
```csharp
public class ReceiptEmailControlViewModel : EmailControlViewModelBase
{
    
    public string Html { get; set; }

}
```

## The Control Type

```csharp
public sealed class ReceiptEmailControlType : EmailControlTypeBase<ReceiptEmailControlData, ReceiptEmailControlViewModel>
{
    public override ReceiptEmailControlViewModel DoBuildViewModel(ReceiptEmailControlData model, EmailControlRenderingData renderingData)
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

Steps
* Add models (Control Type, Data and ViewModel)
* Add HTML-views for backoffice (edit and view)
* Add custom theme-folder
* Add cshtml-view for alias on Control Type.



TODO:
* [ ] Improve step by step guide.
