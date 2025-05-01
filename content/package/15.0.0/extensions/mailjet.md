# Send using Mailjet
A [email service provider](../develop/email-service-providers.md)-implementation that used the [Mailjet](https://www.mailjet.com/) API to send emails in bulk.

## Getting started
Start by adding the extension to your Umbraco project by installing it from [NuGet](https://www.nuget.org/packages/NewsletterStudio.Plugins.Mailjet)

```bash
dotnet add package NewsletterStudio.Plugins.Mailjet
```

After the installation, you need to configure your workspace(s) to use Mailjet for sending emails.

![Configure workspace to use Mailjet](/media/extensions/mailjet/mailjet-workspace-configuration.png)

After this you can send a test-email from any campaign to see if the API-integration is working.

{% hint style="info" %}
License: MIT, source code is available on [Github](https://github.com/enkelmedia/NewsletterStudio.Plugins.Mailjet).
{% endhint %}
