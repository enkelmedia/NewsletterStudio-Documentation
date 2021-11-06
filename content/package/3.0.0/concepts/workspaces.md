# Workspace

Workspaces provides the foundation needed to support multiple tenants inside the same application.

All installations of Newsletter Studio contains one or more Workspaces. They are used to create independent spaces inside the package and each Workspace has it’s own collection of [recipients](../concepts/recipients.md), [mailing lists](../concepts/mailing-lists.md), [campaigns](../concepts/campaigns.md), [transactional emails](../concepts/transactionals.md) and settings.

This is useful for installations that host multiple websites where each website needs its own set data. This would mean that the same recipient-email can exist multiple times but connected to different Workspaces. They are treated as two different entities and no information is shared between Workspaces.

![Workspace settings](/media/administration-workspace-details.png)

Is easy to set up permissions for the Workspace based on backoffice User Groups or on User level.

Only Umbraco users in the Administrator-role can created/edit Workspaces.
