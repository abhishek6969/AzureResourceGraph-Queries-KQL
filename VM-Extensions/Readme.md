# Azure Resource Graph Query for Virtual Machines with Azure Monitor Extension

## Overview

This document provides details on how to use the Kusto query to retrieve and analyze information about Azure virtual machines that have the "AzureMonitorWindowsAgent" extension installed. The query helps you identify virtual machines with this specific extension, along with their OS details and VM sizes.

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
   | where type == 'microsoft.compute/virtualmachines'
   | extend
       JoinID = toupper(id),
       OSName = tostring(properties.osProfile.computerName),
       OSType = tostring(properties.storageProfile.osDisk.osType),
       VMSize = tostring(properties.hardwareProfile.vmSize)
   | join kind=leftouter(
       resources
       | where type == 'microsoft.compute/virtualmachines/extensions'
       | extend
           VMId = toupper(substring(id, 0, indexof(id, '/extensions'))),
           automaticUpgrade = properties.enableAutomaticUpgrade,
           ExtensionName = name
       | where ExtensionName == "AzureMonitorWindowsAgent"
   ) on $left.JoinID == $right.VMId
   | project ExtensionName, id, OSName, OSType, VMSize, automaticUpgrade, OS = properties.extended.instanceView.osName
   | where ExtensionName == "AzureMonitorWindowsAgent"
   | order by tolower(OSName) asc
### Step 3: Download the Results

1. Once the query execution is complete, you will see the results displayed in a table format.
2. To download the results, click on the **Download** button (downward arrow icon) located above the results table.
3. Choose the desired format (CSV, JSON) and save the file to your local machine.

## Output

When you run the query, the output table will include the following fields:

- **ExtensionName**: The name of the extension, which will be "AzureMonitorWindowsAgent" in this case.
- **id**: The unique identifier for each virtual machine.
- **OSName**: The computer name of the virtual machine, representing the OS name.
- **OSType**: The type of the OS disk (e.g., `Windows` or `Linux`).
- **VMSize**: The size of the virtual machine.
- **automaticUpgrade**: Indicates whether automatic upgrade is enabled for the extension.
- **OS**: The name of the operating system as reported by the virtual machine instance view.

The query filters and joins virtual machines with their installed extensions to identify those with the "AzureMonitorWindowsAgent" extension. It provides details about the OS and VM size, helping you to manage and monitor the virtual machines effectively.

## Conclusion

This Kusto query is a powerful tool for identifying Azure virtual machines that have the "AzureMonitorWindowsAgent" extension installed. By running this query in the Azure Resource Graph Explorer, you can efficiently gather and analyze information about these VMs, including their OS details and VM sizes. Additionally, the query can be easily tailored to search for other extensions by modifying the `ExtensionName` filter, allowing you to adapt it for various monitoring or management needs.
