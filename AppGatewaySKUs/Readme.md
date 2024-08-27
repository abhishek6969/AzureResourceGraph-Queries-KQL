# Azure Resource Graph Query for Application Gateways

## Overview

This document provides details on how to use the provided Kusto query to retrieve and analyze information about Azure Application Gateways across your subscriptions. The query classifies Application Gateways based on their SKU version (v1 or v2) and extracts relevant details such as the name, location, SKU type, tier, resource group, and subscription ID. This information can be used for monitoring, auditing, or inventory purposes.

## Usage

### Step 1: Access Azure Resource Graph Explorer

1. Log in to the [Azure Portal](https://portal.azure.com/).
2. Navigate to the **Resource Graph Explorer** by searching for "Resource Graph" in the top search bar.

### Step 2: Run the Query

1. Copy the Kusto query provided below.
2. Paste it into the query editor in the Resource Graph Explorer.
3. Select the relevant subscriptions you want to query against (if not already selected).
4. Click the **Run query** button to execute the query.

### Step 3: Download the Results

1. Once the query execution is complete, you will see the results displayed in a table format.
2. To download the results, click on the **Download** button (downward arrow icon) located above the results table.
3. Choose the desired format (CSV, JSON) and save the file to your local machine.

## Query Details


Resources
| where type == "microsoft.network/applicationgateways"
| extend skuVersion = case(
    properties.sku.tier == "Standard" or properties.sku.tier == "WAF", "v1",
    properties.sku.name == "Standard_v2" or properties.sku.name == "WAF_v2", "v2",
    "Unknown"
)
| project name, location, sku = properties.sku.name, tier = properties.sku.tier, skuVersion, resourceGroup, subscriptionId
| order by skuVersion asc


### Explanation

- **Resources**: This table contains all Azure resources available within the selected subscriptions.
- **where type == "microsoft.network/applicationgateways"**: Filters the results to only include resources of type "Application Gateways."
- **extend skuVersion**: Adds a new column `skuVersion` based on the SKU tier. It categorizes the Application Gateway as `v1` or `v2`.
- **project**: Selects specific columns to include in the final output (name, location, SKU, tier, SKU version, resource group, subscription ID).
- **order by skuVersion asc**: Sorts the results by `skuVersion` in ascending order, grouping v1 gateways before v2.

## Conclusion

This Kusto query is a powerful tool for gaining insights into the Application Gateways deployed across your Azure environment. By running this query in the Azure Resource Graph Explorer, you can easily filter, sort, and export the data for further analysis or reporting.

