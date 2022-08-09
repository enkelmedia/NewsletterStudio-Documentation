---
title: Recipients
description: Documentation about Recipients inside Newsletter Studio
---
# Email Service Providers
The actual work of dispatching/sending an email is performed by an **Email Service Provider** (implements `IEmailServiceProvider`). The core consists of two providers at the moment:

* **SMTP** - Used to send emails via SMTP servers 
* **SMTP Pickup Directory** - Can be used during development/testing to place e-mails in a folder on the development/test-computer.

We also provide some open source implementations of providers that you can use in your project or use as a reference for your own custom implementations:

* Mailjet: https://github.com/enkelmedia/NewsletterStudio.Plugins.Mailjet
* More come, if you have a provider that you want to share - let us know

The Email Service Provider needs to be configured in the Administration-section for each [Workspace](../concepts/workspaces.md).


## Custom Email Service Provider


```csharp
public class CoolEmailCompanyEmailServiceProvider : IEmailServiceProvider
{
    public string Alias => "coolEmail";
    public string DisplayName => "Cool Email";
    public string SettingsView => "~/App_Plugins/CoolEmail/settings.html";
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