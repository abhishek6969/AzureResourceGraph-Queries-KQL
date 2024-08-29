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
```
