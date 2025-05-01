# Umbraco Forms
This extension integrates with [Umbraco Forms](https://umbraco.com/products/add-ons/forms/), it provides two main features:
* Workflow type to send a Transactional Email.
* Workflow type to add a recipient to a Mailing List

## Getting started
Start by adding the extension to your Umbraco project by installing it from [NuGet](https://www.nuget.org/packages/NewsletterStudio.Plugins.UmbracoForms)

```bash
dotnet add package NewsletterStudio.Plugins.UmbracoForms
```

## Sending transactional email

After the installation, you need to configure your workspace(s) to allow the "Umbraco Forms"-[merge field provider](../develop/merge-field-providers.md) for Transactional Emails.

![Configure workspace](/media/extensions/umbraco-forms/workspace-enable-umbraco-forms.png)

When this is configured, you can create **a new transactional email**. In the first step, pick a form as the data model for the email, this will show fields from this form in the editor.

![Fields from selected form](/media/extensions/umbraco-forms/editor-merge-field-all-fields.png)

When your are ready with the email design, to over to the Forms-section and go to the edit-view for your form. Scroll down to "Edit Workflows" and add the "Send Transactional Email"-workflow. Configure the settings and pick your newly created template.

![Pick transactional template](/media/extensions/umbraco-forms/workflow-send-pick-template.png)

## Adding recipient to a mailing list
The workflow "Add to Mailing List" will create a new recipient based on the information entered in the form. When configuring this workflow you need to map your form fields against fields on the recipient. Custom fields are also supported.

![Map fields](/media/extensions/umbraco-forms/workflow-add-map-fields.png)


{% hint style="info" %}
License: MIT, source code is available on [Github](https://github.com/enkelmedia/NewsletterStudio.Plugins.UmbracoForms).
{% endhint %}
