---
topic: "MongoDB: Cloud Atlas Sharing"
desc: "Sharing a Cloud Atlas Setup"
category_prefix	: "MongoDB: "
indent: true
---

If you haven't read the article [/topics/mongodb_cloud_atlas_setup/](/topics/mongodb_cloud_atlas_setup/) yet, read that first.

This article covers how to share MongoDB clusters among an entire team.

1. Create an organization for your team.

   Navigate to <https://cloud.mongodb.com>
   
   In upper left hand corner of page, click on the drop down.  You should see an option `View All Organizations`.  Select this.
   
   Then, you should see a page where there is a button at upper right to `Create New Organization`.  Click this.
   
2. Name your organization, e.g `cs48-s20-s0-t1-org`.   Add the members of your team to the organization using their email
   addresses.
   
3. Now, create a *team* under your organization.  

   You do this by clicking on the `Access Manager` tab at the top of the screen, and then clicking `Create Team`.
   
   This may seem redundant, but it is a necessary step; you can't give 
   *all* members of an organization access to a cluster, but you can give all *team* members access to a cluster.
   And the members of your team have to be part of an organization before they can belong to a team.
   
   So call the team something like `s0-t1-team`, and all all of the members of your team to it.
   
   
   
   
