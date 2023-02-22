# Sending campaigns programmatically
*(Note this applies to version 4.0.5 and later)*

In some scenarios developers want to create and send campaigns using code. This could be a daily batch job or as a reaction to a event in the system.

You can create an instance of the `CampaignEmail`-class and schedule it to be sent using the `ICampaignEmailService`.

## Creating a Campaign based on Umbraco-content
Here is a example of how to programmatically create a new campaign based on a Umbraco content node.

```csharp
public class SendingCampaignsProgrammatically
{
	private readonly INewsletterStudioService _newsletterStudioService;
	private readonly ICampaignEmailService _campaignEmailService;

	public SendingCampaignsProgrammatically(
		INewsletterStudioService newsletterStudioService,
		ICampaignEmailService campaignEmailService
	)
	{
		_newsletterStudioService = newsletterStudioService;
		_campaignEmailService = campaignEmailService;
	}

	public void CreateCampaignForContent(IPublishedContent content)
	{
		// Getting the mailing list from the API. Use the "Mailing List Picker" data type to avoid hard coding values.
		var mailingList = _newsletterStudioService.GetMailingList(Guid.Parse("10e650af-b90a-4792-82d4-b11069a9ec56"));

		var campaign = CampaignEmail.Create("Campaign From Code", mailingList);
		campaign.ThemeAlias = "Default";
		campaign.Sender = new EmailAddress("info@my-company.com", "Company Inc");
		campaign.Subject = "Hallo World-campaign";
		campaign.ContentKey = content.Key;

		// Set date for when to send the campaign. To send right away, use DateTime.Now and the  
		// send out engine will start the sending the email within 15 seconds.
		campaign.ScheduledSendDate = new DateTime(2023, 01, 30, 16, 20, 00);

		var result = _campaignEmailService.Save(campaign);

		if (!result.Success)
		{
			Console.WriteLine("Could not create campaign: " + result.Message);
			foreach (var error in result.ValidationErrors.Errors)
			{
				Console.WriteLine($"Validation error: {error.Property} {error.ErrorAlias} {error.InternalDescription}");
			}
		}

	}
}
```

## Email based on Email Editor
Creating a "block based" email from code is possible but requires you to build up each row, column and control from code. 

Here is an example:


```csharp
public class CreateBlockBasedCampaign
{
	private readonly INewsletterStudioService _newsletterStudioService;
	private readonly ICampaignEmailService _campaignEmailService;

	public CreateBlockBasedCampaign(
		INewsletterStudioService newsletterStudioService,
		ICampaignEmailService campaignEmailService
	)
	{
		_newsletterStudioService = newsletterStudioService;
		_campaignEmailService = campaignEmailService;
	}

		public void CreateBlockBased()
    {
        var mailingList = _newsletterStudioService.GetMailingList(Guid.Parse("10e650af-b90a-4792-82d4-b11069a9ec56"));
			
        var campaign = CampaignEmail.Create("Block based from Code", mailingList);

        campaign.ThemeAlias = "Default";
        campaign.Sender = new EmailAddress("info@my-company.com", "Company Inc");
        campaign.Subject = "Hallo World-campaign";
        campaign.Content.Rows = new List<EmailRow>();
        campaign.Content.Rows.Add(new EmailRow());
        campaign.Content.Rows[0].Columns = new List<EmailColumn>();
        campaign.Content.Rows[0].Columns.Add(new EmailColumn()
        {
            Columns = 4 // indicate that this column takes of 4/4 of the grid columns.
        });
        campaign.Content.Rows[0].Columns[0].Controls = new List<IEmailControl>();
        campaign.Content.Rows[0].Columns[0].Controls.Add(new TextEmailControlData()
        {
            Html = "I was created from code",
            Padding = new Padding(10)
        });
        
        campaign.ScheduledSendDate = new DateTime(2023, 01, 30, 16, 20, 00);

		var result = _campaignEmailService.Save(campaign);

		if (!result.Success)
		{
			Console.WriteLine("Could not create campaign: " + result.Message);
			foreach (var error in result.ValidationErrors.Errors)
			{
				Console.WriteLine($"Validation error: {error.Property} {error.ErrorAlias} {error.InternalDescription}");
			}
		}


	}
}
```

It could be a good idea to use the `Duplicate()`-method of the `ICampaignEmailService` to start from a template created in the email editor and adjust email controls if needed.
