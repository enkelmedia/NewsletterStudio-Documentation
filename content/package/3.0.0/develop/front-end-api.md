---
title: Front end API
description: Documentation about how to use the front end API in Newsletter Studio
---
# Front end API & Newsletter Studio Service
Some of the most common interactions with the Newsletter Studio-API is gathered in a interface called `INewsletterStudioService`, using this you can for example:

* Add recipients
* Subscribe recipients to mailing lists
* Unsubscribe recipients

Just use the built in IOC-container to get an instance of the interface by injection this as a dependency in the constructor of your controller or use any of the service locators like `DependencyResolver.Current` for MVC.

## Adding a recipient programmatically
The package ships with a simple macro to add a recipient to a list, you can find this in:

```
App_Plugins\NewsletterStudio\Views\MacroPartials\NewsletterStudioSignup.cshtml
```

This show cases a simple way to add a recipient from the front end of your site, if you need to do this in your own controller, here is an example:

```csharp
using System;
using System.Web.Mvc;
using NewsletterStudio.Core.Public;
using NewsletterStudio.Core.Public.Models;

public class SubscribeSurfaceController : Umbraco.Web.Mvc.SurfaceController
{
    private readonly INewsletterStudioService _newsletterStudioService;

    public SubscribeSurfaceController(INewsletterStudioService newsletterStudioService)
    {
        _newsletterStudioService = newsletterStudioService;
    }

    public ActionResult Subscribe(SubscribeModel model)
    {
        var defaultMailingListKey = Guid.Parse("ff104e5c-a0c3-4a5b-a0eb-959685036b18");

        var addRecipientRequest = new AddRecipientRequest(model.Email)
            .WithFirstname(model.Firstname)
            .WithLastname(model.Lastname)
            .SubscribeTo(defaultMailingListKey);

        var result = _newsletterStudioService.AddRecipient(addRecipientRequest);

        if (result.Success)
        {
            return Content("Subscribed");
        }
        else
        {
            return Content($"Error: " + result.Message);
        }
        
    }
}
```

The AddRecipientRequest-class uses a fluent API to set data for the operation, here are som of the methods:
* **WithName()**: Expects a full name and tries to parse it into first/last name.
* **WithFirstname()**: Sets the firstname
* **WithLastname()**: Sets the lastname
* **SubscribeTo()**: Subscribes the new recipient to a given Mailing List. (Tip: Use the Mailing List Picker property editor in the backoffice UI if this needs to be a setting).
* **ForWorkspace()**: Sets the Workspace for the new recipient. Only needed when the solution has multiple workspaces.
* **WithCustomField()** Sets a custom field value for the recipient. Alias must match a configured field alias (Workspace settings)
* **WithSource()** Sets the source property for the recipients. Used to track where recipients subscribe, could be "Start page signup form".
