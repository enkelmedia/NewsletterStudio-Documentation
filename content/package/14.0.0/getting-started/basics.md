---
title: The Basics
description: Here we'll outline basics things you need to know to work with Newsletter Studio
---


# The basics

Here we'll outline some of the important basics about Newsletter Studio. Use this as a starting point before reading details about specific features in the package.

## The Workspace
The package is built to support multiple tenants inside the same installation. This is useful when you want to have totally separated environments with their own data inside the same installation. We call these separated environments Workspaces. 

If a Umbraco installation hosts multiple websites for separate companies or for branches inside a company, each company/branch could have its own Workspace in Newsletter Studio to work independently of each other since a Workspace will have its own collection of recipients, mailing lists, campaigns, and transactional e-mails.

When installing the package a new Workspace is automatically created.

![Screenshot of the Workspace Administration](/media/workspaces-overview.png?width=1380&quality=100)

## Recipients and mailing lists
A recipient is stored once per workspace. When adding or importing a new recipient to a mailing list, the recipient is created and a relation between the recipient and the mailing list is also created.

In older versions of the package, removing a mailing list would also remove any recipients in the list - this is not the case anymore as only, the relation between the list and the recipient will be removed but the recipient will not.


