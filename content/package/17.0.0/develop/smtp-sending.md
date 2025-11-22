---
title: Sending with SMTP
description: Configure Newsletter Studio to send using SMTP and read bounces using POP3.
---
# Sending using SMTP
The default option in Newsletter Studio is to send emails using the SMTP protocol. Most email service providers like SendInBlue, Mailgun,Mailjet, Amazon SES support this protocol.

## Rate Limits
Be aware that some SMTP replay services apply rate limits for how fast you are allowed to send via their service. If your provides enforces restrictions you can use our rate limit feature when you configure the SMTP to repect the rates of your provider.


## Bounce management
If you want to handle bounces for your emails you can provide a mailbox where the package can look for these bounce emails. The package will check periodically and update the state of the recipients and any tracking information if it detects a bounce.

The logic for detecting a bounce is based on the industry standards and look for the `status`-header in the email. This header should contain a number indicating any issues. We also look at the headers `diagnostic-code` and `final-recipient` to figure out if the email is infact a bounce and who was the intended recipient. The "bounce-email" is sent from the intended recipients mail server and not all mail servers follow standards. This mean that the package might be unable to parse certain bounce emails, the is natual as we don't want to set a recipient as bounced if we're not 100% sure.

You can override our logic for determine bounces using the `BounceDetector.OnDetectingBounce`-event handler like this:

```csharp
using NewsletterStudio.Core.Mail.Common;
using Umbraco.Cms.Core.Composing;

public class HandleBouncesComposer : IComposer
{
    public void Compose(IUmbracoBuilder builder)
    {
        BounceDetector.OnDetectingBounce += (object sender, OnDetectingBounceEventArgs args) =>
        {
            // Inspect message
            if (args.Message.RawContent.Contains("invalid recipient"))
            {
                var recipientEmail = "recipient@example.com";

                // Set OverrideResult to override the result 
                // from the built in bounce parser.
                args.OverrideResult = new BounceResult()
                {
                    IsHardBounce = true,
                    FinalRecipient = recipientEmail
                };
            }

        };
    }
}
```
