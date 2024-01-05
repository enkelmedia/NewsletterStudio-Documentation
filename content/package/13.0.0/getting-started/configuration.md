---
title: Configuration
description: How to configure Newsletter Studio for Umbraco
---
# Configuration
## Different types of configuration

Most settings are tied to a [Workspace](../concepts/workspaces.md) and some configuration/settings is only available for administrators, that are settings like permissions, custom fields, site url, email delivery, and so on. Other settings like default senders, Google Analytics, and unsubscribe settings can be made available for editors as well.

## Administrator Configuration

Configure a Workspace by navigating to `Administration -> [Workspace] -> Manage`.

Here you can configure Workspace-level settings for:

* Permissions for Users and User Groups
* Custom Fields
* Providers and Email Editor
* Site Url
* Email Service Provider
  * SMTP
    * Host and authentication
    * Security / TLS
    * Rate Limiting
  * SMTP Pickup
  * [Custom Email Service Provider](../develop/email-service-providers.md)

## Workspace Settings

Each workspace has a "Settings"-icon in the tree, navigate to this to configure settings on the workspace.

Here you can configure

* Default sender for the Workspace
* Tracking settings for Google Analytics (UTM)
* Double opt in-settings
* Unsubscribe-settings

## Configuration files
*Introduced in v13.0.2*

It's also possible to set some settings using `appsettings.json`. This way you can easily adjust settings depending on your environment. For example you can use a certain [Email Service Provider](../develop/email-service-providers.md) for development/testing but ensure that the live-configuration is applied in production.

Not all settings can be applied this way, but you can:

* Set the `Base URL`
* Set Email Service Provider and it's settings

Here is a samle configuration:

```json
{
  "NewsletterStudio" : {
    "Workspaces" : [
      {
        "Key" : "<your guid workspace key>",
        "BaseUrl" : "https://www.lorem-ipsum.com",
        "EmailServiceProvider" : {
          "Alias" : "smtp",
          "Settings" : {
            "host" : "smtp.lorem-ipsum.com",
            "username" : "send-box",
            "password" :  "lorem",
            "useSsl" : true,
            "port" : 587,

            "rateLimitActive" : true,
            "rateLimitItems" : 10,
            "rateLimitSeconds" : 10,

            "bounceActive" : true,
            "bounceHost" : "bounce.lorem-ipsum.com",
            "bounceUsername" : "bounce-box",
            "bouncePassword" : "lorem",
            "bounceUseSsl" : true,
            "bouncePort" : 995,
          }
        }
      }
    ]
  }
}
```

or 

```json
{
  "NewsletterStudio" : {
    "Workspaces" : [
      {
        "EmailServiceProvider" : {
          "Alias" : "smtpPickup",
          "Settings" : {
            "path" : "c:\temp"
          }
        }
      }
    ]
  }
}
```

You can remove the `Workspace.Key`-property to make the settings apply to all workspaces.

Changes to configuration files will be applied after a application restart. When using these configuration files, configured settings are no longer editable in the UI of the backoffice.

## Configuration using code
*Introduced in v13.0.2*

It is also possible to provide the configuration using code by implementing `INewsletterStudioOptionsAccessor` and returning an instance of `NewsletterStudioOptions`. The default implementation of this interface is the class `NewsletterStudioOptionsAccessor` is a singleton that just reads the appsettings but you could replace this with your own implementaiton if needed.

Here is a simple sample:

```csharp
public class SiteNewsletterStudioOptionsAccessor : INewsletterStudioOptionsAccessor
{
    private readonly NewsletterStudioOptions _options;

    public SiteNewsletterStudioOptionsAccessor()
    {
        _options = new NewsletterStudioOptions();
        _options.Workspaces = new List<NewsletterStudioWorkspaceOptions>()
        {
            new NewsletterStudioWorkspaceOptions()
            {
                BaseUrl = "http://www.foobar.com",
            }
        };

    }

    public NewsletterStudioOptions Value => _options;
}

// register:

services.AddSingleton<INewsletterStudioOptionsAccessor,SiteNewsletterStudioOptionsAccessor>();

```