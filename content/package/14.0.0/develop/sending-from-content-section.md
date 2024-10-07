# Sending from the Content Section
Newsletter Studio supports sending Umbraco content as campaigns right from the content section via our Content App-extension. After settings this up editors can create newsletters that are listed on the website and then with a click of a button send this as a campaign using Newsletter Studio.

![Sending campaigns from the Content-section](/media/campaign-content-section.png)

This would also allow you to restrict access to the Newsletter Studio-section for non administrators and only give editors access to the Content App in the content section.

## Setting it up
By default, any content type with the alias "newsletter" will activate the Content App to send from the content section. If you want to show the content app on another content type, just configure this in `appsettings.json` by setting `NewsletterStudio:CampaignContentTypes` to a comma-separated string of Content Type Aliases. 

Like this:

```json
{
    "NewsletterStudio" : {
        "CampaignContentTypes" :  "myCampaignContentTypeAlias,anotherCampaignContentTypeAlias"
    }
}    
```

### Configure using code
It's also possible to configure the content type aliases using code during startup, here is an example that uses a `IComposer`

```csharp
using Umbraco.Cms.Core.DependencyInjection;

public class AppStart : Umbraco.Cms.Core.Composing.IComposer
{
    public void Compose(IUmbracoBuilder builder)
    {
        NewsletterStudio.Core.Public.NewsletterStudioService.ContentAppDocumentTypeAliases.Add("myDocumentTypeAlias");
    }
}
```

## Configure the template
By default Newsletter Studio will use the selected Template of the content node to render the HTML for the email. Most of the time we probably want to use a custom template to render the HTML for the email. Just create a new template with the same name but append `Newsletter` in the end.

For example, let's say we have a Content Type called `Article` with a template called `Article`. To use a custom template for the email we would add a new template called `ArticleNewsletter` like this:

![campaigns-overview](/media/sending-from-content-section-template-name.png)

We also need to allow this template in the Content Type configuration:

![campaigns-overview](/media/sending-from-content-section-template-allow.png)

With this setup Newsletter Studio will use the `ArticleNewsletter`-template to render the HTML for the e-mail.

## Permissions
 Note that editors need to have the "ContentApp"-permissions for the Workspace, as an administrator you’ll always have access to all Workspaces/Features. This can be configured in Administration -> (Your Workspace) -> Permissions. If there are no permissions configured everyone can access everything.

