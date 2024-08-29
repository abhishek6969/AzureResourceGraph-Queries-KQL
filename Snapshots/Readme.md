# Azure Resource Graph Query for Snapshot Details with Subscription Names

## Overview

This document provides details on how to use the Kusto query to retrieve and analyze information about Azure snapshots, including their IDs, creation times, and associated subscription names. This query helps in correlating snapshot data with subscription details for better management and reporting.

## Usage

### Step 1: Access Azure Resource Graph Explorer

1. Log in to the [Azure Portal](https://portal.azure.com/).
2. Navigate to the **Resource Graph Explorer** by searching for "Resource Graph" in the top search bar.

### Step 2: Run the Query

1. Copy the Kusto query provided below.
2. Paste it into the query editor in the Resource Graph Explorer.
3. Click the **Run query** button to execute the query.

   ```kusto
   // Query to get snapshots with their IDs and creation time
   resources
   | where type == "microsoft.compute/snapshots"
   | project id, name, subscriptionId, resourceGroup, timeCreated = properties.timeCreated
   | join kind=inner (
       // Query to get subscription names
       ResourceContainers
       | where type =~ 'microsoft.resources/subscriptions'
       | project subscriptionId = id, subscriptionName = name
   ) on subscriptionId
   | project id, name, subscriptionName, resourceGroup, timeCreated
   
## Output

When you run the query, the output table will include the following fields:

- **id**: The unique identifier for each snapshot.
- **name**: The name of the snapshot.
- **subscriptionName**: The name of the subscription to which the snapshot belongs.
- **resourceGroup**: The resource group in which the snapshot is located.
- **timeCreated**: The creation time of the snapshot.

The query joins snapshot details with subscription information to display the subscription names along with each snapshot's ID, creation time, and resource group. This data is useful for managing and reporting on snapshots across different subscriptions.

## Conclusion

This Kusto query is a powerful tool for extracting and correlating snapshot details with subscription names in your Azure environment. By running this query in the Azure Resource Graph Explorer, you can efficiently identify and analyze snapshots in relation to their subscriptions, enhancing your resource management and reporting capabilities.

