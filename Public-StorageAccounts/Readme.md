# Azure Resource Graph Query for Azure Storage Accounts with Public Network Access

## Overview

This document provides instructions on how to use the Kusto query to retrieve and analyze information about Azure Storage accounts that have public network access allowed. This query helps identify storage accounts configured to allow access over the public internet, which is crucial for security audits and compliance.

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
   | where type == 'microsoft.storage/storageaccounts'
   | project name, resourceGroup, publicNetworkAccess = properties.networkAcls.defaultAction, ['kind']
   | where publicNetworkAccess == 'Allow'
\```

### Step 3: Download the Results

Once the query execution is complete, you will see the results displayed in a table format.  
To download the results, click on the **Download** button (downward arrow icon) located above the results table.  
Choose the desired format (CSV, JSON) and save the file to your local machine.

## Output

When you run the query, the output table will include the following fields:

- **name**: The name of the storage account.
- **resourceGroup**: The resource group in which the storage account is located.
- **publicNetworkAccess**: Indicates whether public network access is allowed. The value will be 'Allow' if public access is enabled.
- **kind**: The type of storage account, such as 'StorageV2'.

The query filters the results to show only those storage accounts where public network access is set to 'Allow'. This information is useful for identifying and reviewing storage accounts that may have potential security risks due to public accessibility.

## Conclusion

This Kusto query is a valuable tool for assessing the network access configuration of Azure Storage accounts. By running this query in the Azure Resource Graph Explorer, you can efficiently identify which storage accounts are configured to allow public network access and take appropriate actions to secure your resources.

