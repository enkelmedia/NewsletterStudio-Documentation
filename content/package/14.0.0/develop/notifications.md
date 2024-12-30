---
title: Notifications / Events
description: Documentation about Events inside Newsletter Studio
---
# Notifications / Events
Newsletter Studio uses the build in Notification mechanism in Umbraco to fire notification events. 

Here's a simple example of a notification handler that kicks in just be for a [Email Service Provider](./email-service-providers.md) sends a email.
```csharp
public class EmailSendingHandler : INotificationHandler<EmailSendingNotification>
{
    public void Handle(EmailSendingNotification notification)
    { 
        // The Message-parameter, of type object, will be different 
        // concrete type depending on which EmaiL Service Provider that is used.
        if (notification.Message is MailMessage legacyMessage)
        {
            legacyMessage.Subject = "Glenn";
        }

        if (notification.Message is MimeMessage mimeMessage)
        {
            mimeMessage.Subject = "Glenn";
        }
    }
}
```

Please refer to the [official Umbraco documentation](https://docs.umbraco.com/umbraco-cms/14.latest/fundamentals/code/subscribing-to-notifications) for detailed instructions on how to setup a notification handler.

Here's a list of the current events that we're providing, we're working to add more events, feel free to reach out if you're missing something.


#### EmailSendingNotification
Fired by a [Email Service Provider](./email-service-providers.md) just before a e-mail is sent. The parameter will be the concrete e-mail message type for this provider.
<hr/>

#### TreeRenderingNotification
Fired when the Newsletter Studio-tree has been rendered. Use this to change the menu items in the tree
