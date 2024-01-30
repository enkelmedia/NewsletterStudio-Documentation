---
title: Configuration
description: How to configure Newsletter Studio for Umbraco
---
# Configuration
## Different types of configuration

Most settings are tied to a [Workspace](../concepts/workspaces.md) and some configuration/settings are only available for administrators, that is settings like permissions, custom fields, site url, email delivery, and so on. Other settings like default senders, Google Analytics, and unsubscribe settings can be made available for editors as well.

## Administrator Configuration

Configure a Workspace by navigating to `Administration -> [Workspace] -> Manage`.

Here you can configure Workspace-level settings for:

* Permissions for Users and User Groups
* Custom Fields
* Providers and Email Editor
* Site url
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
* Settings for List-Unsubscribe headers.

## Configuration files
*Introduced in v3.0.14*

Version 3 of the package only exposes a small subset of file based configuration options to be configured in `web.config`.

Here is a samle configuration:

```xml
<appSettings>
  <add key="NewsletterStudio:CampaignContentTypes" value="exampleContentTypeAlias,anotherContentTypeAlias" />
  <add key="NewsletterStudio:Debug:LogCampaignLinks" value="true" />
  <add key="NewsletterStudio:Debug:DebugFilesPath" value="c:\\temp\\debug\\" />
</appSettings>
```

You can also provide overrides for things like `Email Service Provider` and `Workspace Base URL` but in version 3 this requires some custom code. A potential solution is to have a environment-setting in `web.config` and then use this to return different values depending on the environment using a custom `INewsletterStudioOptionsAccessor`-implementaiton.

## Configuration using code
*Introduced in v3.0.14*

It is also possible to provide the configuration using code by implementing `INewsletterStudioOptionsAccessor` and returning an instance of `NewsletterStudioOptions`. The default implementation of this interface is the class `NewsletterStudioOptionsAccessor` is a singleton that just reads some of the settings in `web.config` but you could replace this with your own implementaiton if needed.

Here is a simple sample:

```csharp
public class SiteComposer : IUserComposer
{
    public void Compose(Composition composition)
    {
        composition.Register<INewsletterStudioOptionsAccessor,SiteNewsletterStudioOptionsAccessor>(Lifetime.Singleton);
    }
}

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
```