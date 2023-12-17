---
title: Merge Field Providers
description: Documentation about Merge Field Providers inside Newsletter Studio
---
# Merge Field Providers
The email editor supports merge fields, that is placeholders for dynamic content that will be replaced during the rendering. This makes it very easy for editors to [insert dynamic content inside the editor](../concepts/email-editor#merge-fields).

Some of the built in merge fields are:
* {{email}} - Replaced by the recipients email
* {{firstname}} - Replaced by the recipients first name
* {{lastname}} - Replaced by the recipients last name

Each "Custom Field" on a recipient can also be inserted as a merge field in the email editor.

**Note** that merge fields will not be fully replaced during preview-rendering since the merge fields are dependent on the recipient and there is no recipient during preview. It's recommended to send a real email to a test-recipient to perform a realistic test.

![Merge field picker inside editor](/media/editor-merge-field-picker.png)

## Extending Merge Fields
It's possible to add custom merge fields to the email editor, this is useful if you create a custom integration and would like to append more dynamic fields that the editor can choose from so that they don't have to remember the exact placeholder. A custom merge field provider must implement `ICampaignEmailMergeFieldProvider`or `ITransactionalEmailMergeFieldProvider` depending on what context it's needed for.

Many times a Merge Field Provider is created togheter with a [Recipient List Provider](../develop/recipient-list-providers.md)

### Example

```csharp
public class InMemoryMergeFieldProvider : ICampaignEmailMergeFieldProvider
{
    public string Alias => "inMemory";
    public string DisplayName => "In memory";

    public List<MergeField> GetFields(Guid workspaceKey)
    {
        return new List<MergeField>()
        {
            new MergeField()
            {
                Text = nameof(InMemoryRecipient.CompanyName),
                Placeholder = "companyName",
                GroupText = "In Memory"
            },
            new MergeField()
            {
                Text = nameof(InMemoryRecipient.City),
                Placeholder = "city",
                GroupText = "In Memory"
            },
            new MergeField()
            {
                Text = nameof(InMemoryRecipient.BirthYear),
                Placeholder = "birthYear",
                GroupText = "In Memory"
            }
        };
        
    }

    public List<MergeFieldValue> ExtractValues(CampaignMergeFieldValuesRequestModel request)
    {
        var list = new List<MergeFieldValue>();

        if (request.RecipientProviderRecipientDataModel.Model is InMemoryRecipient recipient)
        {
            list.Add(new MergeFieldValue("companyName",recipient.CompanyName));
            list.Add(new MergeFieldValue("city",recipient.City));
            list.Add(new MergeFieldValue("birthYear",recipient.BirthYear.ToString()));
        }

        return list;

    }

}
```

This also needs to be registered during startup:

```csharp
public class MySiteComposer : IComposer {
    public void Compose(IUmbracoBuilder builder)
    {
        builder.NewsletterStudio().CampaignMergeFieldProviders.Append<InMemoryMergeFieldProvider>();
    }
}
```

**Remember** to also activate the Merge Field Provider in the Workspace-settings (Administration -> Workspace -> Manage)

