# Workspace

Workspaces provide the foundation needed to support multiple tenants inside the same application.

All installations of Newsletter Studio contains one or more Workspaces. They are used to create independent spaces inside the package. Each Workspace has it's own collection of [recipients](../concepts/recipients.md), [mailing lists](../concepts/mailing-lists.md), [campaigns](../concepts/campaigns.md), [transactional emails](../concepts/transactionals.md) and settings.

This is useful for installations that host multiple websites where each website needs its own set of data. This would mean that the same recipient email can exist multiple times, but connected to different Workspaces. They are treated as two different entities and no information is shared between Workspaces.

![Workspace settings](/media/administration-workspace-details.png)

Is easy to set up permissions for a Workspace based on backoffice User Groups or a User level.

Only Umbraco users in the Administrator role can create/edit Workspaces.

## Workspace Key
The workspace key is a unique identifier for a workspace, this key is used by the license to verify your workspace. 

To find your workspace key, go to `Administration` and click on your workspace.

![Workspace settings](/media/workspaces-administration-list.png)

Then look for your **workspace key** in the top right corner.

![Workspace settings](/media/workspaces-workspace-key.png)
 
