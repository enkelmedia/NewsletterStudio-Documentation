---
title: Recipient List Providers
description: Documentation about Recipient Providers inside Newsletter Studio
---
# Recipient Providers
These providers expose lists and recipients for Campaigns. For example, the lists shown when sending a campaign are all fetched from our built in Recipient List Providers.

![Choose recipients when sending campaigns](/media/campaigns-choose-recipients.png)

## Custom Recipient Provider
Developers can create custom Recipient Providers to expose lists from any custom source. That could be a CRM, webshop, custom database table, or any other data source.

The custom class needs to implement `IRecipientListProvider` and needs to be registered during startup. It's also common to implement a [Merge Field Provider](../develop/merge-field-providers.md) to work in conjunction with the Recipient List Provider. This is required if you want to use merge fields from your custom Recipient List Provider inside emails.

The custom provider will be used to show lists as options in the Send-step of the campaign editor, and also to provide data for the send out queue. There are no built in way to show/list/browse the recipients in a custom Recipient List Provider - only the built in Mailing Lists are shown the package UI. Most of the time there are already existing UI for the source (think of Umbraco Members) but if you need this you can always extend Umbraco to implement custom dashboards/views to show your data.

### Example
Here is a example of a custom Recipient List Provider with hardcoded / in memory values.

A working Umbraco-website with this provider can be found in our [contrib-project](https://github.com/enkelmedia/NewsletterStudioContrib/tree/master/Newsletter%20Studio%20V14/Extensions-Demos).

Most of the types and methods have XML-comments and documentation - use your IDE and hover types/methods to read more information.

{% contrib file="V16/Extensions-Demos/Demo.Web/Extensions/RecipientListProvider/InMemoryRecipientListProvider.cs" %}
{% endcontrib %}

This sample uses a small POCO that looks like this:

{% contrib file="V16/Extensions-Demos/Demo.Web/Extensions/RecipientListProvider/InMemoryRecipient.cs" %}
{% endcontrib %}

We also need to register the provider during startup:

{% contrib file="V16/Extensions-Demos/Demo.Web/Extensions/RecipientListProvider/InMemoryRecipientListProviderComposer.cs" %}
{% endcontrib %}

After this we need to run the website, and go to Administration -> Workspace to allow our new custom provider for Campaigns:

![Activate provider in Workspace-admin](/media/develop--administration-workspace-recipient-list-providers.png)
