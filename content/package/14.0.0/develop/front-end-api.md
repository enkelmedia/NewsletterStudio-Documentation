---
title: Front end API
description: Documentation about how to use the front end API in Newsletter Studio
---
# Front end API and Newsletter Studio Service
Some of the most common interactions with the Newsletter Studio-API is gathered in an interface called `INewsletterStudioService`.

With this, you can for example:

* Add recipients
* Subscribe recipients to mailing lists
* Unsubscribe recipients
* Send vanilla emails using configured [Email Service Provider](../develop/email-service-providers.md)

Just use the built-in IOC-container to get an instance of the interface by injecting it as a dependency into your controller.

## Adding a recipient programmatically
The package ships with a simple demo-view that shows to add a recipient to a list, you can find it here:

`\Views\Partials\NewsletterStudioSignup.cshtml`

This showcases a simple way to add a recipient from the front end of your site, if you need to do this in your own controller, here is an example:

{% contrib file="V14/Extensions-Demos/Demo.Web/Features/AddRecipient/AddRecipientController.cs" %}
{% endcontrib %}

The AddRecipientRequest-class contains a fluent API to set data for the operation, here are some of the methods:
* **WithName()**: Expects a full name and tries to parse it into first/last name.
* **WithFirstname()**: Sets the first name
* **WithLastname()**: Sets the last name
* **SubscribeTo()**: Subscribes the new recipient to a given Mailing List. (Tip: Use the Mailing List Picker property editor in the backoffice UI if this needs to be a setting).
* **ForWorkspace()**: Sets the Workspace for the new recipient. Only needed when the solution has multiple workspaces.
* **WithCustomField()** Sets a custom field value for the recipient. The alias must match a configured field alias (Workspace settings)
* **WithSource()** Sets the source property for the recipients. Used to track where recipients subscribe, could be i.g "Homepage", "Footer form" or any string.

## Sending transactional email programmatically
The service is also when you want to send transactional emails, please have a look at the [transactional email guide](../concepts/transactionals.md) for a complete example.

## Updating subscription statuses for recipients
It's possible to update any subscription status from the service as well. 

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

## Custom Unsubscribe-page
It's possible to create a custom page where you can handle the unsubscribe-process. This page can for example use the API in `INewsletterStudioService` to list the current subscriptions for the recipient.

First, configure the "Unsubscribe confirmation url" in the `Settings` under the Workspace.

When this is configured we'll route recipients that want to unsubscribe to this page and append a token to the URL.

`https://www.mypage.com/custom-unsubscribe?token=acb123.....`

When rendering this page you can use the `ParseUnsubscribeToken(string token)`-method of `INewsletterStudioService` to get information about the recipient eg. the RecipientKey. Further down the process you can pass the RecipientKey to the `GetSubscriptionsFor()`, `RemoveRecipient()` or `Unsubscribe()`-methods.

## Other useful methods
There is plenty of other useful methods on the INewsletterStudioService.

* **SendTransactionalAsync()** - Sends a transactional email.
* **SendMailMessageAsync()** - Sends a standard email (created by your code) using the configured [Email Service Provider](../develop/email-service-providers.md).
* **Unsubscribe()** - Globally unsubscribes a recipient
* **GetRecipientsByEmail()** - Get's all recipient by the provided email. Note that one e-mail can exist multiple times if using multiple workspaces.
* **GetSubscriptionsFor()** - Get's all mailing list-subscriptions for a given recipient.
* **IsValidEmail()** - Checks if a string is a valid e-mail address.
* **GetMailingListsForAllWorkspaces()** - Gets all mailing lists for all Workspaces.
* **RemoveRecipient()** - Removes a recipient and makes tracking data anonymized.
