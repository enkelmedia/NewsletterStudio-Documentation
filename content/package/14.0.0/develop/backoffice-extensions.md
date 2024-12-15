---
title: Backoffice extensions
description: Overview of backoffice extensions for Newsletter Studio
---
# Backoffice extensions
With the introduction of "the new backoffice" aka. Bellissima in Umbraco v14, the way that backoffice extensions are built drastically changed.

The old AngularJS-implementation was thrown away and a new, modern, implementation using Web Components was introduced. We'll refer you to the [umbraco documentation](https://docs.umbraco.com/umbraco-cms/14.latest/customizing/development-flow) to read about the basics of setting up a development environment.

You can also use our [NewsletterStudioContrib](https://github.com/enkelmedia/NewsletterStudioContrib)-project as a starting point - it has all the setup you need.

Newsletter Studio comes with a couple of extension points for the backoffice, here are a overview of the extension type aliases we support

* `nsEmailControlEditUi` used when creating a [email control](../develop/email-control.md).
* `nsEmailControlDisplayUi` used when creating a [email control](../develop/email-control.md).
* `nsAction` - used to inject actions (like remove, duplicate, export etc) for different entities like nsRecipient, nsMailingList etc.
* `nsEmailServiceProviderSettingsUi` - optionally used when building a custom [Email Service Provider](../develop/email-service-providers.md).

