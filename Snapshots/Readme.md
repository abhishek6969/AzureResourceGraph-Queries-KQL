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

