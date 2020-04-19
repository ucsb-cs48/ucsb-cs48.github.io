---
topic: "MongoDB: Cloud Atlas Setup"
desc: "Setting up MongoDB Cloud Atlas (for new users)"
category_prefix	: "MongoDB: "
indent: true
---

The current recommended MongoDB installation for CS48 is MongoDB Cloud Atlas.

The instructions in this article cover how to set up a new free account, up to the step where you get a MongoDB URI connection string.

# Step 1: Create free account

  Navigate to <https://mongodb.com/cloud/atlas> and click the "Start&nbsp;Free" button.

  ![00-mongo-db-atlas-start-free.png](00-mongo-db-atlas-start-free-30.png)

# Step 2: Fill out the form

  Fill in the form. We encourage you to use your `@ucsb.edu` account.

  ![01-get-started-free-form.png](01-get-started-free-form-30.png)

# Step 3: Choose the Free Plan

  The next page asks you to choose from among various plans.  Naturally,
  you should choose the free plan, so click the "Create&nbsp;a&nbsp;cluster"
  button in that column.
  
  ![03-choose-account-type.png](03-choose-account-type-30.png)

  ![04-choose-free-plan.png](04-choose-free-plan-30.png)

# Step 4: Choose a cloud provider

  MongoDB Cloud Atlas runs, under the hood, on other cloud providers: Amazon, Google, and Microsoft's platforms.   The default is Amazon, and you need to choose either US East, or US West.

  ![05-choose-cloud-provider.png](05-choose-cloud-provider-30.png)

  If you are located in California, there may be some marginal performance benefit to choosing US West, 

  ![06-select-us-west.png](06-select-us-west-30.png)

# Step 5: Cluster Creation

  ![07-cluster-creation.png](07-cluster-creation-50.png)

  ![08-cluster-creation-done.png](08-cluster-creation-done-50.png)

  ![09-connect-to-cluster.png](09-connect-to-cluster-50.png)

# Step 6: Whitelist Current IP Address

  ![10-whitelist-current-ip-address.png](10-whitelist-current-ip-address-50.png)

# Step 7: Create a MongoDB User

  ![11-create-a-mongodb-user.png](11-create-a-mongodb-user-50.png)

  ![12-ready-to-connect.png](12-ready-to-connect-50.png)

# Step 8: Obtain the URI string for connection 

  ![13-connect-your-application.png](13-connect-your-application-50.png)

  ![14-connection-string-node-3.0.png](14-connection-string-node-3.0-50.png)

  ![15-database-access-menu-item.png](15-database-access-menu-item-50.png)

  ![16-database-access-screen.png](16-database-access-screen-50.png)

# Step 9: Obtaining (changing) the DB User Password

  ![17-edit-user.png](17-edit-user-50.png)

  ![18-change-db-user-password.png](18-change-db-user-password-50.png)

  ![19-edit-mongodb-uri-with-password-holder.png](19-edit-mongodb-uri-with-password-holder-50.png)

  ![20-copy-password.png](20-copy-password-50.png)

  ![21-paste-in-password.png](21-paste-in-password-50.png)
