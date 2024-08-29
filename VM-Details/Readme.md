# Azure Resource Graph Query for Virtual Machine Details

## Overview

This document provides instructions on how to use the Kusto query to retrieve and analyze detailed information about Azure virtual machines. The query collects data on virtual machine configurations, including OS details, image versions, network interfaces, and subscription information.

## Usage

### Step 1: Access Azure Resource Graph Explorer

1. Log in to the [Azure Portal](https://portal.azure.com/).
2. Navigate to the **Resource Graph Explorer** by searching for "Resource Graph" in the top search bar.

### Step 2: Copy and Load the Query

1. Download the query file from the repository..
2. Open the file and copy the entire query.
3. Paste the query into the query editor in the Resource Graph Explorer.
4. Click the **Run query** button to execute the query.

### Step 3: Download the Results

1. Once the query execution is complete, you will see the results displayed in a table format.
2. To download the results, click on the **Download** button (downward arrow icon) located above the results table.
3. Choose the desired format (CSV, JSON) and save the file to your local machine.

## Output

When you run the query, the output table will include the following fields:

- **vmName**: The name of the virtual machine.
- **subscriptionName**: The name of the subscription to which the virtual machine belongs.
- **privateIPAddress**: The private IP address associated with the virtual machine’s network interface.
- **osType**: The type of the OS disk (e.g., `Windows` or `Linux`).
- **resourceGroup**: The resource group in which the virtual machine is located.
- **vmSize**: The size of the virtual machine.
- **location**: The Azure region where the virtual machine is deployed.
- **osName**: The name of the operating system running on the virtual machine.
- **osVersion**: The version of the operating system.
- **imageID**: The ID of the image used to create the virtual machine.
- **imageVersion**: The version of the image used.

The query joins virtual machine data with network interface and subscription information to provide a comprehensive view of VM configurations and network details. This data is useful for managing and auditing virtual machines across different subscriptions.

## Conclusion

This Kusto query is a valuable tool for extracting detailed configuration and network information about Azure virtual machines. By running this query in the Azure Resource Graph Explorer, you can efficiently gather insights into your VM environments, helping with management, auditing, and reporting activities.


