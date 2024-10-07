---
title: Macro
description: Documentation about macros inside Newsletter Studio
---
# Email Control: Macro
The Macro-control makes it possible to include dynamic content, generated with a `cshtml` file, in both Campaign and Transactional Emails.

This can be used to generate recipient-specific content like discounts, promotions, personalized content or render a receipt. When using a Macro you have 100% control over the rendered HTML.

## Creating a Macro
Macros can be created in two ways, either as a Global or as a Theme Specific.

### Global
When you want a Macro to be usable everywhere in the email editor, just add a cshtml-file like this:

`App_Plugins/newsletterStudioExtensions/views/macros/myNewMacro.cshtml`

### Theme-specific
A theme-specific Macro only works when this theme is used. To create a Theme-specific macro add a file like this:

`App_Plugins/mySite/newsletterStudio/themes/myTheme/views/macros/myNewMacro.cshtml`

## Example of Macro-content

```cshtml
@model NewsletterStudio.Core.Editor.Controls.Macro.EmailMacroViewModel

<p>Demo cshtml-Macro Rendering</p>

<strong>Email</strong><br/>
@Model.RecipientDataModel.Email

<strong>Merge Fields</strong><br/>
<ul>
    @foreach (var field in Model.RecipientDataModel.MergeFields)
    {
        <li>
            @field.Placeholder: @field.Value
        </li>
    }
</ul>

<strong>Transactional Model</strong><br/>
Has Model: @(Model.TransactionalModel == null ? "No" : "Yes")

@if (Model.TransactionalModel != null)
{
    <text>Model Type: @Model.TransactionalModel.GetType() </text>
}

@* Example of how to look for a given Transactional Model if the macro needs to read data from the model *@

@if (Model.TransactionalModel is ForgotPasswordModel data)
{
    <p>Data from model: @data.Data.Name</p>
}
```

## Injecting dependencies
Views inside a macro supports injecting dependencies using `@inject`, just like any other razor-view. Here is an example of how to access the UmbracoContext from within a macro:

```cshtml
@using Umbraco.Cms.Core.Web
@inject IUmbracoContextFactory UmbracoContextFactory
@{
    var pageName = "";
    using (var ctx = UmbracoContextFactory.EnsureUmbracoContext())
    {
        pageName = ctx.UmbracoContext.Content.GetById(1057).Name;
    }
}

<h2>@pageName</h2>
```

**Be aware:** Do not use accessors like `IUmbracoContextAccessor` or `IHttpContextAccessor` inside macros as they are rendered in a background thread during send out. Make sure to send your campaigns/transactional emails to a test SMTP-server or SMTP Pickup Directory to make sure that rendering works as expected. Do not rely on previews as they are rendered by a HTTP-request hence might have access to dependencies that are not accessible in a background-thread.