---
title: Mailing Lists
description: Documentation about Mailing Lists inside Newsletter Studio
---
# Mailing Lists
The Mailing Lists are used to group [recipients](../concepts/recipients.md) into different lists. Each recipient can be connected to zero or more lists and the connection also as a `Status`, the status can be `Pending`, `Subscribed` or `Unsubscribed`. When using "Double Opt-In" new recipients will have a `Pending` connection to the Mailing List until the subscription is confirmed.

![email-editor--edit](/media/mailing-lists--overview.png)

## Importing recipients
In the `Actions`-menu in the top right corner, you'll find the import-wizard that makes it easy to import recipients. You can either paste a comma or semicolon-separated list or recipients or upload a CSV-file.

<video src="/media/import-recipients-paste.mp4" autoplay loop muted playsinline width="100%" />

Video showing how to import recipients.

## Double Opt In
The concept of double opt in means that any new recipient would have to confirm the subscription before they are added to any mailing list. When activated the workflow would be:

* A visitor signs up for a mailing list
* A new recipient is added to the mailing list with the status `Pending` to inciate that we're waiting for confirmation.
* A transactional email is sent to the recipient to complete the sign-up. This email contains a confirmation-link.
* When the link is clicked the recipient-status is changed to `Active`.

This feature is configured in the `Settings` for each Workspace like this:

* In tree to the left, click on `Settings`
* Scroll to the "Double opt in"-section and check the "Enable Double-Opt-in"-checkbox. 
* Choose a custom template for the transactional email, or use the default one.
* Enter a URL in the `Confirmation Url` if you want to show a custom landing page when the recipient clicks the confirmation-link in the e-email.
  
After this any new signups via [the frontend api](../develop/front-end-api.md) the frontend api will be using the workflow above to confirm the subscription.

