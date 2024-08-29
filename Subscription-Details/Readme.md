# Azure Resource Graph Query for Subscription Information

## Overview

This document provides instructions on how to use the Kusto query to retrieve and analyze information about Azure subscriptions. The query helps in listing all the Azure subscriptions in your environment.

## Usage

### Step 1: Access Azure Resource Graph Explorer

1. Log in to the [Azure Portal](https://portal.azure.com/).
2. Navigate to the **Resource Graph Explorer** by searching for "Resource Graph" in the top search bar.

### Step 2: Run the Query

1. Copy the Kusto query provided below.
2. Paste it into the query editor in the Resource Graph Explorer.
3. Click the **Run query** button to execute the query.

   ```kusto
   ResourceContainers 
   | where type =~ 'microsoft.resources/subscriptions'
### Step 3: Download the Results

1. Once the query execution is complete, you will see the results displayed in a table format.
2. To download the results, click on the **Download** button (downward arrow icon) located above the results table.
3. Choose the desired format (CSV, JSON) and save the file to your local machine.

## Output

When you run the query, the output table will include the following fields:

- **id**: The unique identifier for each subscription.
- **name**: The name of the subscription.

The query retrieves basic details about the subscriptions present in your Azure environment. This information is useful for identifying and managing subscriptions.

## Conclusion

This Kusto query is a straightforward tool for listing all Azure subscriptions within your environment. By running this query in the Azure Resource Graph Explorer, you can easily view and manage your subscriptions, which is essential for organizational and administrative purposes.
