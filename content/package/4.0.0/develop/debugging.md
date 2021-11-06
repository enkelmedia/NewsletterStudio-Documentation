---
title: Debugging
description: Documentation about debugging Newsletter Studio-issues
---
# Debugging issues

Some configuration settings

### Log Campaign Links and issues with link tracking
This settings will activate a logger that outputs all links from sent emails to a text file, it will also log any errors from TrackingControllerActions reg. parsing the tokens. Configuration if done in `appSettings.json`.

Values: true / false (default: false)

```json
  "NewsletterStudio" : {    
    "Debug" : {
      "LogCampaignLinks" : true
    }
  }
```

### Debug File Path
The path to any debug files should be created.

Value: Example: c:\temp (default: c:\temp)
```json
  "NewsletterStudio" : {    
    "Debug" : {
      "LogCampaignLinks" : true,
      "DebugFilesPath" : "c:\\temp",
    }
  }
```

### Logging
We log to the Umbraco Log (in the backoffice: Settings / Log Viewer). The majority of our messages is logged with the priority "Debug" so do debug using the log, make sure that your configuration is correct.

In your `appSettings.json`-file, make sure the following lines are present to view debug-messages.

```json
  "Serilog": {
    "MinimumLevel": {
      "Default": "Debug"
    }
  },
```


You can also filter the log items in the Log Viewer with this search query to only see stuff related to the package:

```xml
SourceContext like '%Newsletter%' OR @Message like '%Newsletter%'
```