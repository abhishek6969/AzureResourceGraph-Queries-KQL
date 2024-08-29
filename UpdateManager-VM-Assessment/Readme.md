# Azure Resource Graph Query for Azure Update Manager Assessment for VMs

## Overview

This document provides details on how to use the provided Kusto query to retrieve and analyze information about VM patch status using update manager.
## Usage

### Step 1: Access Azure Resource Graph Explorer

1. Log in to the [Azure Portal](https://portal.azure.com/).
2. Navigate to the **Resource Graph Explorer** by searching for "Resource Graph" in the top search bar.

### Step 2: Run the Query

1. Copy the Kusto query provided in repository.
2. Paste it into the query editor in the Resource Graph Explorer.
3. Click the **Run query** button to execute the query.

### Step 3: Download the Results

1. Once the query execution is complete, you will see the results displayed in a table format.
2. To download the results, click on the **Download** button (downward arrow icon) located above the results table.
3. Choose the desired format (CSV, JSON) and save the file to your local machine.
## Output

When you run the query, the output table will include the following fields:

- **resourceId**: The unique identifier for each Azure VM.
- **name** : The name of each Azure VM.
- **assessProperties**: Contains detailed assessment information for each VM.

The `UMStatus` field in the output table will reflect the patch status of each VM based on the following conditions:

- **VM not running**: If the VM is not running and either the assessment properties are null or the assessment status is "inprogress", and the VM status is not "VM running".
- **Not Assessed**: If the VM is running and either the assessment properties are null or the assessment status is "inprogress".
- **Patched**: If the VM is assessed and the OS is Windows with no available critical or security patches, or the OS is Linux with no available security patches, and the assessment status is neither "inprogress" nor unsupported.
- **Not Patched**: If the VM is assessed and the OS is Windows with available critical or security patches, or the OS is Linux with available security patches, and the assessment status is neither "inprogress" nor unsupported.
- **Unsupported**: If the VM uses an unsupported marketplace image or if the detected VM guest patch support state is "Unsupported".

This information allows you to efficiently understand and manage the patch status of VMs across your Azure environment.

## Conclusion

This Kusto query serves as a valuable tool for extracting detailed information about the inventory of resources within your Azure environment. When executed in the Azure Resource Graph Explorer, this query allows you to efficiently filter, organize, and export data pertaining to various Azure resources.



