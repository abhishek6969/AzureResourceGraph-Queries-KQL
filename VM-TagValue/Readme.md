# Azure Resource Graph Query for Virtual Machines with Tags and Subscription Details

## Overview

This document provides details on how to use the Kusto query to retrieve and analyze information about Azure virtual machines, including their subscription details, operating system, and tags. The query filters virtual machines by specific subscription IDs and includes details about associated tags.

## Usage

### Step 1: Access Azure Resource Graph Explorer

1. Log in to the [Azure Portal](https://portal.azure.com/).
2. Navigate to the **Resource Graph Explorer** by searching for "Resource Graph" in the top search bar.

### Step 2: Run the Query

1. Copy the Kusto query provided below.
2. Paste it into the query editor in the Resource Graph Explorer.
3. Click the **Run query** button to execute the query.

   ```kusto
   resources
   | where type == "microsoft.compute/virtualmachines"
   | where subscriptionId in ("your-sub-id1","your-sub-id2")
   | extend OS = properties.extended.instanceView.osName
   | extend SCALEID = properties.virtualMachineScaleSet.id
   | project id, name, subscriptionId, resourceGroup, OS, SCALEID
   | join kind=inner (
       resourcecontainers
       | where type == "microsoft.resources/subscriptions"
       | project subscriptionId, SubscriptionName=name
   ) on subscriptionId
   | project id, name, SubscriptionName, resourceGroup, OS, SCALEID
   | join kind=leftouter (
       resources
       | where type == "microsoft.compute/virtualmachines"
       | where subscriptionId in ("1392dc2d-cfa1-4a86-a9f1-27200e320c38","31854b7f-0c6b-49b0-817e-7211ab9a688d","32137a4d-e897-4d54-ac18-bed5a746009e","5384e520-ec1a-4d72-bed2-0ace565e6cde","586e730f-cdad-486f-8986-f656c60c019e","730e19c6-9a35-444f-984f-0c0b5051e05b","dc30cfe8-6546-4932-8d08-9604c0825cba")
       | mvexpand tags
       | extend tagKey = tostring(bag_keys(tags)[0])
       | extend tagValue = tostring(tags[tagKey])
       | where tagKey == "lg-sid"
       | project id, name, tagKey, tagValue
   ) on id
   | extend tagValue = coalesce(tagValue, "No Tag found for SID")
   | project id, name, SubscriptionName, resourceGroup, OS, SCALEID, tagValue
### Step 3: Download the Results

1. Once the query execution is complete, you will see the results displayed in a table format.
2. To download the results, click on the **Download** button (downward arrow icon) located above the results table.
3. Choose the desired format (CSV, JSON) and save the file to your local machine.

## Output

When you run the query, the output table will include the following fields:

- **id**: The unique identifier for each virtual machine.
- **name**: The name of the virtual machine.
- **SubscriptionName**: The name of the subscription to which the virtual machine belongs.
- **resourceGroup**: The resource group in which the virtual machine is located.
- **OS**: The operating system name of the virtual machine.
- **SCALEID**: The ID of the virtual machine scale set, if applicable.
- **tagValue**: The value of the "lg-sid" tag associated with the virtual machine, or "No Tag found for SID" if the tag is not present.

The query filters virtual machines by specific subscription IDs and joins this information with subscription details and tags to provide a comprehensive view of the virtual machines.

## Conclusion

This Kusto query is a powerful tool for extracting detailed information about Azure virtual machines, including their subscription details and tags. By running this query in the Azure Resource Graph Explorer, you can efficiently analyze and manage virtual machines across different subscriptions. Additionally, the query can be tailored to include different subscription IDs or tags as needed, making it versatile for various reporting and management tasks.
