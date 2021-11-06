---
title: Render Tasks
description: Documentation about Render Tasks inside Newsletter Studio
---
# Render Tasks
Using a Render Task a developer can hook into the rendering process of the email and make changes to the message body/subject and to the MailMessage-object it self. This can be used to add attachments, custom headers or to replace/update content in the email.

Newsletter Studio uses Render Tasks to:
* Replace merge fields (ReplaceMergeFieldsRenderTask)
* Append tracking to links (AddClickTagsRenderTask)
* Insert tracking pixel in email (AddTrackingPixelRenderTask)
* Make relative links absolute (ReplaceRelativeLinksRenderTask)
* Replace Umbraco-links and relative images (ProcessUmbracoContentRenderTask)


## Custom Render Task
Developers can create render tasks and register them during startup. Just implement `IRenderTask`, we will call the `Process()` method once for each recipient. Be aware that any heavy work inside a Render Task will slow down the rendering and send out process.


```csharp
public class AddAttachmentRenderTasks : IRenderTask
{
    public void Process(RenderTaskProcessingResult result, RenderTaskParameters parameters)
    {
        parameters.MailMessage.Attachments.Add(GetAttachmentFor(parameters.Recipient));
    }

    private Attachment GetAttachmentFor(IRecipientDataModel recipientDataModel)
    {
        using (var stream = File.OpenRead("c:\\temp\\invoice.pdf"))
        {
            return new Attachment(stream,"Invoice123.pdf");
        }
    }
}
```

We also need to register the Render Task during startup

```csharp
public class SiteComposer : IUserComposer
{
    public void Compose(Composition composition)
    {
        composition.NewsletterStudio().RenderTasks.Append<AddAttachmentRenderTasks>();
    }
}
```

## Remove Core tasks
It's also possible to remove/replace the core Render Tasks, here is a example on how to inactivate all the tracking:

```csharp
public class MySiteComposer : IUserComposer
{
    public void Compose(Composition composition)
    {
        composition.NewsletterStudio().RenderTasks.Remove<AddTrackingPixelRenderTask>();
        composition.NewsletterStudio().RenderTasks.Remove<AddClickTagsRenderTask>();
    }
}
```

This will of course mean that any reports will not work.

