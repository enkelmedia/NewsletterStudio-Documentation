---
title: Debugging
description: Documentation about debugging Newsletter Studio-issues
---
# Debugging issues

Some configuration settings

### Log Campaign Links and issues with link tracking
This settings will active a debugger that outputs all links from send outs to a text file, it will also log any errors from TrackingControllerActions reg. parsing the tokens.

Values: true / false (default: false)

```
<add key="NewsletterStudio.Debug.LogCampaignLinks" value="true" />
```

### Debug File Path
The path to any debug files should be created.

Value: Example: c:\temp (default: c:\temp)
```
<add key="NewsletterStudio.Debug.DebugFilesPath" value="true" />
```
