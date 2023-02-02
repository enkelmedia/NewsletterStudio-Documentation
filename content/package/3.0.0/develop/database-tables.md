---
title: Database tables
description: Documentation about database tables
---
# Database tables

Here we'll outline some internals of how we store data in the database. This can be useful for debugging or when you need to create customizations that the offical API does not support (at your own risk).

We do not see the database design as a contract so we might add or change tables/columns in future releases. However, we always try to "append only".

First of all, most tables have a column for `WorkspaceKey` this is used when there are multiple [Workspaces](../getting-started/basics.md) in use. 

### nsWorkspace
Contains the configured Workspaces for a installation.

### nsWorkspaceAccess
Contains any configured user access settings.

### nsMailingList

Contains the built-in mailing lists for the package.

### nsRecipient

Stores recipients that are added to any of the built-in mailing lists. If you're sending to Umbraco Members this table does not hold any data about them.

### nsSubscription

A relation table between nsMailingList and nsRecipient, holds information about which recipient is subscribed (or unsubscribed) from which list.

### nsCampaignEmail

This table contains all campaigns created. The `status` field indicates if it is a draft, if it sending is in progress, or if it's sent.

Statuses:

* 0 = Created / Draft
* 1 = Initializing (send out engine is creating nsTrackingCampaignEmail-rows for the campaign)
* 2 = Sending
* 3 = Sent
* 4 = Error

### nsTrackingCampaignEmail

This table acts as a queue when sending a campaign email and also as a log for statistic reports. It will contain unique recipients for the campaign and any bounce information for that recipient. 

Be aware that it's technically possible that one recipient (unique email) is represented in multiple [Recipient List Providers](../develop/recipient-list-providers.md) used when sending the email. The send-out engine will make sure that only one unique recipient for each campaign is stored here.

Statuses:

* 0 = Available (sending not started to this recipient)
* 1 = InProgress
* 2 = Error
* 3 = Sent


### nsTrackingCampaignEmailInteraction

This table stores any interactions from the recipient like open & click. It also stores a URL when applicable (e.g. which URL was clicked).

Tracking Types:

* 1 = Open
* 2 = Click
* 3 = Unsubscribe Click
* 4 = Unsubscribe Completed
* 5 = View in browser

### nsTransactionalEmail
This tables stores all transactional emails with it's content and settings.

### nsTrackingTransactional
This tables stores a row each time a transactional email is sent. Since a transactional email can be sent to multiple recipients each recipient is stored in ´nsTrackingTransactionalEmail`

### nsTrackingTransactionalEmail
This table holds a row for each unique email sent when sending a transactional email.

### nsTrackingTransactionalInteraction
This table holds the interactions performed by a recipient of a transactional email.

### nsKeyValue
A key-value store that we use to store random information e.g. about installation.