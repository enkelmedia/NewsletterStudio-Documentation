---
title: Transactional Emails
description: Documentation about Transactional Emails with Newsletter Studio for Umbraco
---
# Transactional Emails
With the support for Transactional Emails, implementers can use our email editor for all kinds of emails that the site needs to send. This could be a "sign up confirmation", "forgot password" or a receipt from a webshop.

![Screenshot of the Transactional Emails](/media/transactional-list.png?width=1380&quality=100)

## How it works

By defining a model with the `TransactionalEmail` attribute, our editor will pick up any available fields and present them for editors in a simple drop down.

![Screenshot of email editor with fields](/media/v15/transactional-edit-fields.png)

You can also configure the package to track and log any email that has been sent, providing analytics insights and a historic log.

![Screenshot of transactional email log](/media/v15/transactional-log.png)

From the log, you can drill into the details about a specific email

![Screenshot of email details](/media/v15/transactional-history.png)

You can even see the content of the individual email and resend it if needed.

![Screenshot of email details](/media/v15/transactional-view.png)


## Hooking it up
Start by creating a model that defines the email you want to send.

```csharp
[TransactionalEmail("Forgot Password", Alias)]
public class ForgotPasswordEmailModel
{
    public const string Alias = "forgotPassword";
    
    [MergeField("Member-number","memberNumber")]
    public string MemberNumber { get; set; }
    
    [MergeField("First name","firstName")]
    public string FirstName { get; set; }
    
    [MergeField("Email","email")]
    public string Email { get; set; }
    
    [MergeField("ConfirmationLink","confirmationLink")]
    public string ConfirmationLink { get; set; }
}
```

After the model has been created you need to create new a "Transactional Email"-template for this model. Startup the site, go to the Email-section -> Transactionals -> Emails and create a new Transactional Email. Click on the "Design Email"-button and follow the steps, make sure to choose `Forgot Password` as Data Model for the template. Edit the email and save the changes.

Then we need to trigger a sending of the email, this is done using the `INewsletterStudioService`. Just inject this into the controller, component, or background job that needs to trigger transactional email.

```csharp
[Route("SendTransactional/{action}/{id?}")]
public class SendTransactionalController : Controller
{
    private readonly INewsletterStudioService _newsletterStudioService;

    public SendTransactionalController(INewsletterStudioService newsletterStudioService)
    {
        _newsletterStudioService = newsletterStudioService;
    }
    
    public ActionResult Test()
    {
        var model = new ForgotPasswordEmailModel();
        model.Email = "john@lorem-ipsum.com";
        model.FirstName = "John";
        model.MemberNumber = "123";
        model.ConfirmationLink = "https://www.lorem-ipsum.se/confirm/dsdff947kjdfg92mkfsd92";

        _newsletterStudioService.SendTransactional(
            SendTransactionalEmailRequest.Create(model)
                .SendTo(model.Email)
                .WithSubject("Hallo")
                .Build()
        );
            
            
        return Content("It works");
    }
}
```

We'll automatically look for a matching transactional email template based on the type of model passed with the `SendTransactionalEmailRequest` but to be safe we recommend that you create a settings-node and use the `Transactional Email Picker`-data type to make it possible to select the template to use.
