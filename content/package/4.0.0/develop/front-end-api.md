---
title: Front end API
description: Documentation about how to use the front end API in Newsletter Studio
---
# Front end API and Newsletter Studio Service
Some of the most common interactions with the Newsletter Studio-API is gathered in a interface called `INewsletterStudioService`, using this you can for example:

* Add recipients
* Subscribe recipients to mailing lists
* Unsubscribe recipients

Just use the built in IOC-container to get an instance of the interface by injecting it as a dependency in the constructor of your controller or use any of the service locators like `DependencyResolver.Current` for MVC.

## Adding a recipient programmatically
The package ships with a simple macro to add a recipient to a list, you can find this in:

```xml
\App_Plugins\NewsletterStudio\Views\MacroPartials\NewsletterStudioSignup.cshtml
```

This show cases a simple way to add a recipient from the front end of your site, if you need to do this in your own controller, here is an example:

```csharp
public class SubscribeSurfaceController : SurfaceController
{
    private readonly INewsletterStudioService _newsletterStudioService;

    public SubscribeSurfaceController(INewsletterStudioService newsletterStudioService)
    {
        _newsletterStudioService = newsletterStudioService;
    }

    public ActionResult Subscribe(SubscribeModel model)
    {
        var defaultMailingListKey = Guid.Parse("ff104e5c-a0c3-4a5b-a0eb-959685036b18"); // Replace with your mailing list key
        
        var request = AddRecipientRequest.Create("john.doe@foobar.com")
            .ForWorkspace(Guid.Parse("606D0954-DDAE-4ADF-BCB6-DD113ADD915E")) // Optional, only used with multiple workspaces
            .WithFirstname("John")
            .WithLastname("Doe")
            .WithSource("Website")
            .WithCustomField("city", "London")
            .SubscribeTo(defaultMailingListKey)
            .Build();

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

The AddRecipientRequest-class contains a fluent API to set data for the operation, here are some of methods:
* **WithName()**: Expects a full name and tries to parse it into first/last name.
* **WithFirstname()**: Sets the firstname
* **WithLastname()**: Sets the lastname
* **SubscribeTo()**: Subscribes the new recipient to a given Mailing List. (Tip: Use the Mailing List Picker property editor in the backoffice UI if this needs to be a setting).
* **ForWorkspace()**: Sets the Workspace for the new recipient. Only needed when the solution has multiple workspaces.
* **WithCustomField()** Sets a custom field value for the recipient. Alias must match a configured field alias (Workspace settings)
* **WithSource()** Sets the source property for the recipients. Used to track where recipients subscribe, could be ie "Homepage", "Footer form" or any string.

## Sending transactional email programmatically
The service is also when you want to send transactional emails, please have a look at the [transactional email guide](../concepts/transactionals.md) for a complete example.

## Updating subscription statuses for recipient
It's possible to update any subscription-status from the service as well. 

```csharp
var recipientKey = Guid.Parse("1495412D-538C-480E-BAF2-C747D33A3E8B");
var mailingList1Key = Guid.Parse("5EC42DB0-E46D-4966-A5B4-3B4C3C578B98");
var mailingList2Key = Guid.Parse("A7D0BDAF-7BC9-4035-B65E-DDF81816103E");
            
var request = AddOrUpdateSubscriptionsRequest.Create()
    .Set(recipientKey,mailingList1Key,SubscriptionStatus.Subscribed)
    .Set(recipientKey,mailingList2Key,SubscriptionStatus.Subscribed)
    .Build();

var result = _newsletterStudioService.AddOrUpdateSubscriptions(request);
```

## Other useful methods
There is plenty of other useful methods on the INewsletterStudioService.

* **SendMailMessage()** - Sends a standard .NET MailMessage with the configured [Email Service Provider](../develop/email-service-providers.md).
* **Unsubscribe()** - Globally unsubscribes a recipient
* **GetRecipientsByEmail()** - Get's all recipient by the provided email. Note that one e-mail can exist multiple times if using multiple workspaces.
* **GetSubscriptionsFor()** - Get's all mailing list-subscriptions for a given recipient.
* **IsValidEmail()** - Checks if a string is a valid e-mail address.
* **GetMailingListsForAllWorkspaces()** - Gets all mailing lists for all Workspaces.