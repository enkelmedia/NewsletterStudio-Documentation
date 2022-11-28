# Sending from the Content Section
If you want to use a content node as the source for your newsletter you can use the included Content App. This can be very useful if you for example want to store all newsletters as content on your website. This means that you don't have to copy and paste the text - just send it directly from the content section.

![Sending campaigns from the Content-section](/media/campaign-content-section.png)

Or if you don't want to give your editors access to the Newsletter Studio section, you could use the Content App-approach and then remove their access to the section, so they only send Campaigns from the content section.

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

### Legacy-way
To activate the feature you need to tell Newsletter Studio about the Document Type's where the Content App should be visible. This is done using code during start-up.
In version 3 you need to append any DocumentTypeAlias to a static list

The best way to run this code is using a custom Composer

```csharp
using Umbraco.Core.Composing;
using Umbraco.Web;

public class AppStart : Umbraco.Core.Composing.IUserComposer
{
    public void Compose(Composition composition)
    {
        NewsletterStudio.Core.Public.NewsletterStudioService.ContentAppDocumentTypeAliases.Add("myDocumentTypeAlias");
    }
}
```

You can put this where ever you like inside your project or even in the "/App_Code"-folder if you do not compile your own dll's.
 
Editors also need to have the "ContentApp"-permissions for the Workspace, as an administrator you’ll always have access to all Workspaces/Features. This can be configured in Administration -> (Your Workspace) -> Permissions. If there are no permissions configured everyone can access everything.
