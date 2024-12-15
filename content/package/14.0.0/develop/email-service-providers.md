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

```csharp
public class CoolEmailCompanyEmailServiceProvider : IEmailServiceProvider
{
    public string Alias => "coolEmail";
    public string DisplayName => "Cool Email";
    public string SettingsView => "~/App_Plugins/coolEmail/settings.html";
    public Dictionary<string, object> Settings { get; set; }
    
    public SendOutConfiguration GetSendOutConfiguration()
    {
        throw new NotImplementedException();
    }

    public ValidationErrorCollection ValidateSettings(Dictionary<string, object> settings)
    {
        throw new NotImplementedException();
    }

    public void Send(List<SendEmailJob> batch)
    {
        throw new NotImplementedException();
    }

    public CommandResult Send(MailMessage message)
    {
        throw new NotImplementedException();
    }
}
```

Adding the Email Service Provider to our list of services, in your startup code:


```csharp
public class MySiteComposer : IComposer {
    public void Compose(IUmbracoBuilder builder)
    {
        builder.NewsletterStudio().EmailServiceProviders.Append<CoolEmailCompanyEmailServiceProvider>();
    }
}
```

Then, in the Workspace administration, the new provider should show up here:

![Configure custom provider](/media/administration--allow-email-service-provider.png)