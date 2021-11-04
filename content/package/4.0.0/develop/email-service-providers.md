---
title: Recipients
description: Documentation about Recipients inside Newsletter Studio
---
# Email Service Providers
The actual work of dispatching/sending an email is performed by a **Email Service Provider** (implements `IEmailServiceProvider`). The core consists of two providers at the moment:

* **SMTP** - Used to send emails via SMTP servers 
* **SMTP Pickup Directory** - Can be used during development/testing to place e-mails in a folder on the development/test-computer.

The Email Service Provider needs to be configred in the Administration-section for each [Workspace](../concepts/workspaces.md).


## Custom Email Service Provider

TODO:
* [ ] How to create custom? 123
