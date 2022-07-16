---
title: Recipient List Providers
description: Documentation about Recipient Providers inside Newsletter Studio
---
# Recipient Providers
These providers expose lists and recipients for Campaigns, it's a Recipient Provider that shows the Umbraco Member groups when choosing recipients for a Campaign. 

![Choose recipients when sending campaigns](/media/campaigns-choose-recipients.png)

## Custom Recipient Provider
Developers can create custom Recipient Providers to expose lists from any custom source That could be a CRM, a webshop, a custom database table or any other data source.

The custom class needs to implement `IRecipientListProvider` and needs to be registered during startup. It's also common to implement a [Merge Field Provider](../develop/merge-field-providers.md) to work in conjuntion with the Recipient List Provider.

### Example

```csharp
public class InMemoryRecipientListProvider : IRecipientListProvider
{
    public string DisplayName => "Demo Provider";
    public string DisplayNameLocalizationKey => "site_demoProvider";
    public string Prefix => "demo";
    public bool CanRedirectToEdit => false;

    public List<RecipientList> GetLists(Guid workspaceKey)
    {
        return new List<RecipientList>()
        {
            new RecipientList()
            {
                Identifier = new RecipientListIdentifier(Prefix, 1),
                Name = "Demo list 1",
                Subscribers = 25
            },
            new RecipientList()
            {
                Identifier = new RecipientListIdentifier(Prefix, 2),
                Name = "Demo list 2",
                Subscribers = 21
            }
        };
    }

    public List<EmailReceiver> GetReceiversForList(RecipientListIdentifier listId, GetReceiversForListParams parameters)
    {
        var list = new List<EmailReceiver>();
        list.Add(new EmailReceiver(new EmailReceiverIdentifier(Prefix,1),"foo@bar.com","Foo Bar"));

        if(listId.Identifier == "2")
            list.Add(new EmailReceiver(new EmailReceiverIdentifier(Prefix,2),"foo2@bar.com","Foo Bar"));

        return list;
    }

    public EmailReceiver GetByEmail(string email, Guid? workspaceKey = null)
    {
        throw new NotImplementedException();
    }

    public RecipientProviderRecipientDataModel GetDataModel(EmailReceiverIdentifier receiverId)
    {
        var model = new RecipientProviderRecipientDataModel();
        model.Email = "foo@bar.com";
        model.ProviderRecipientId = receiverId.ToString();

        // The "Model"-property is used to pass a custom model that can be used inside 
        // a IMergeFieldProvider to translate properties into Merge Fields.
        model.Model = new InMemoryRecipient()
        {
            BirthYear = 1975,
            City = "London",
            CompanyName = "Test Company Inc."
        };

        return model;
    }

    public bool Unsubscribe(EmailReceiverIdentifier receiverId, RecipientListIdentifier listId)
    {
        //TODO: Change status
        return true;
    }

    public bool UnsubscribeAll(EmailReceiverIdentifier receiverId)
    {
        //TODO: Change status
        return true;
    }

    public string GetEditUrl(EmailReceiverIdentifier receiverId)
    {
        // Used when CanRedirectToEdit is true to allow for redirection to a edit-view. 
        // Ig. the Umbraco Member-view for Umbraco members.
        throw new NotImplementedException();
    }
}
```

We also need to register the provider during startup:

```csharp
public class MySiteComposer : IComposer {
    public void Compose(IUmbracoBuilder builder)
    {
        builder.NewsletterStudio().RecipientListProviders.Append<InMemoryRecipientListProvider>();
    }
}

```

