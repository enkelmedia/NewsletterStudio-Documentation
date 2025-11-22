---
title: Custom Email Service Provider
description: Documentation about Email Service Providers inside Newsletter Studio
---
# Email Service Providers
The actual work of dispatching/sending an email is performed by an **Email Service Provider** (implements `IEmailServiceProvider`). Some providers requires settings, this can be implemented using a extension for the backoffice.

The package ships with four providers out of the box:

* **SMTP** - Sends emails using MailKit
* **SMTP (Umbraco)** - A MailKit-implementation that will reuse the Umbraco-settings in appSettings.json.
* **SMTP (Legacy)** - Uses the [obsoleted](https://learn.microsoft.com/en-us/dotnet/api/System.Net.Mail.SmtpClient?view=net-8.0#remarks) `SmtpClient` in System.Net.Mail.
* **SMTP Pickup Directory** - Can be used during development/testing to place e-mails in a folder on the development/test-computer.

We also provide some open source implementations of providers that you can use in your project or use as a reference for your own custom implementations:

* Mailjet: https://github.com/enkelmedia/NewsletterStudio.Plugins.Mailjet
* More come, if you have a provider that you want to share - let us know

The Email Service Provider needs to be configured in the Administration-section for each [Workspace](../concepts/workspaces.md).

## Custom Email Service Provider
Use some of the open source providers above as inspiration. Here is a simple example of a "empty" provider:

{% contrib file="V16/Extensions-Demos/Demo.Web/Extensions/EmailServiceProvider/CoolCompanyEmailServiceProvider.cs" %}
{% endcontrib %}


Adding the Email Service Provider to our list of services, in your startup code:

{% contrib file="V16/Extensions-Demos/Demo.Web/Extensions/EmailServiceProvider/CoolCompanyComposer.cs" %}
{% endcontrib %}


Then, in the Workspace administration, the new provider should show up here:

![Configure custom provider](/media/administration--allow-email-service-provider.png)

## Adding UI for configuration
You can also provide an extension for the backoffice to mount a element for settings related to the provider.

First, create a element to render

{% contrib file="V16/Extensions-Demos/Demo.Web/Client/src/email-service-provider/cool-email-email-service-provider-settings.element.ts" %}
{% endcontrib %}

Then register the element as a `nsEmailServiceProviderSettingsUi` extension.

{% contrib file="V16/Extensions-Demos/Demo.Web/Client/src/email-service-provider/manifest.ts" %}
{% endcontrib %}