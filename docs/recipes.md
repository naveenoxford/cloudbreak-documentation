## Recipes

[comment]: <> (This is from the old UI. Need to rewrite when new UI is ready.)

Although Cloudbreak lets you provision HDP clusters in the cloud based on custom Ambari blueprints, Cloudbreak provisioning options don't consider all possible use cases. For that reason, we introduced recipes. 

A recipe is a script that runs on all nodes of a selected node group before or after the Ambari cluster installation. You can use recipes for tasks such as installing additional software or performing advanced cluster configuration. For example, you can use a recipe to put a JAR file on the Hadoop classpath.

When creating a cluster, you can optionally upload one or more "recipes" (custom scripts) and they will be executed on a specific host group before or after the cluster installation. 


### Writing Recipes

When using recipes, consider the following:

* The scripts will be executed on the node types you specify (such as "master", "worker", "compute"). If you want to run a a script on all nodes, define the recipe one per node type.  
* The script will execute on all of the nodes of that type as root.  
* In order to be executed, your script must be in a network location which is accessible from the cloud controller and cluster instances VPC.  
* Make sure to follow Linux best practices when creating your scripts. For example, don't forget to script "Yes" auto-answers where needed.  
* Do not execute yum update –y since it may update other components on the instances (such as salt), which can create unintended or unstable behavior.   
* The scripts will be executed as root. The recipe output is written to `/var/log/recipes` on each node on which it was executed.
 

#### Sample Recipe for Yum Proxy Setting

```
#!/bin/bash
cat >> /etc/yum.conf <<ENDOF
proxy=http://10.0.0.133:3128
ENDOF
```

### Adding Recipes

To add a recipe, perform these steps.

**Steps**

1. Place your script in a network location accessible from Cloudbreak and cluster instances virtual network. 
  
2. Select the recipe when creating a cluster using the Cloudbreak UI. You must provide:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your recipe. |
| Description | (Optional) Enter a description for your recipe.|
| Execution Type | Select **PRE** or **POST**, depending on whether you want the script to be executed prior to or post Ambari cluster deployment. |
| Script | <p>Select one of: <ul><li>**Script**: Paste the script.</li><li> **File**: Point to a file on your machine that contains the recipe.</li><li> **URL**: Specify the URL for your recipe.</li></ul> |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this recipe to create clusters, but they cannot delete it. | 

    Example: 

    <a href="../images/recipe-add.png" target="_blank" title="click to enlarge"><img src="../images/recipe-add.png" width="650" title="Cloudbreak web UI"></a> 
    
3. When creating a cluster, select **Show Advanced Options** > **Choose Blueprint** and specify which recipe you want to execute on which host group. 

    Example: 

    <a href="../images/recipe-select.png" target="_blank" title="click to enlarge"><img src="../images/recipe-select.png" width="650" title="Cloudbreak web UI"></a> 


### Deleting Recipes

You can delete previously added items by selecting and item and using the **delete** option. 

### Modifying Recipes 

To modify previously added recipes:

* If you pasted or uploaded your recipe in the Cloudbreak UI, in order to modify it you must delete the entry and add the recipe again in a new entry.   
* If you provided a URL to the location where the recipe is stored, in order to modify the recipe simply update it in the location to which the URL is pointing.       


[comment]: <> (TO-DO: Move Shell commands to the Cb Shell doc.) 
