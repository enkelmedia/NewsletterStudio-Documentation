# Sending to Umbraco Members
The package has built in support for sending to Umbraco members based on the `Member Group` they are associated with. Here is a example of a Umbraco website with the Member Groups "Customers", "Gold Customers" and "Silver Customers".

![Choose recipients when sending campaigns](/media/campaigns-choose-recipients.png)

If you have `Member Groups` configured you can start sending to members right away. However, to be able to give the recipients the option to unsubscribe you need to create a "magic" property on the `Member Type`.

## Facilitate Unsubscribe
Go to the Settings-section of the backoffice and open your `Member Type`, add a new property to the member type.

**Name:** Wants Newsletter?

**Alias:** emailCampaigns

**Type:** true/false (boolean)

The `Alias` and the `Type` are important but feel free to name the property what every you like.

Then this property exists we will only send campaigns to Umbraco Members that have the `emailCampaigns`-property set to true. We will also switch this property if the recipient clicks on any unsubscribe-links in the campaigns.

If you need a more fine granied setup or logic you can always create your own [Recipient List Provider](../develop/recipient-list-providers.md) and disable the built in provider in the Workspace-administration.