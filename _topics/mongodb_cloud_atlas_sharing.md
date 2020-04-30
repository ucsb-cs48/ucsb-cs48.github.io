---
topic: "MongoDB: Cloud Atlas Sharing"
desc: "Sharing a Cloud Atlas Setup"
category_prefix	: "MongoDB: "
indent: true
---

If you haven't read the article [/topics/mongodb_cloud_atlas_setup/](/topics/mongodb_cloud_atlas_setup/) yet, read that first.

This article covers how to share MongoDB clusters among an entire team.

This assumes you are starting from scratch, creating new database clusters for your team.   You can also follow
these instructions using the existing organization rather than creating a new one; if you already have a database established
and want to keep using it, that may be a better approach.

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
   
4. Now, with your organization selected, click `Projects`, the first item in the left hand sidebar menu.
   
   Under Projects, you can create a new cluster, and follow all the steps under [/topics/mongodb_cloud_atlas_setup/](/topics/mongodb_cloud_atlas_setup/) to create a new cluster on which to store MongoDB databases.
   
   You can then, on the Projects tab, click in the `Users` or `Teams` columns to give individual organization users,
   or the entire team, access to the project.
   
5. Change the MongoDB credentials (the URI containing the username/password) in your app to use the new shared
   MongoDB database.
   
   
   
   
