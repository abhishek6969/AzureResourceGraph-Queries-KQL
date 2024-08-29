# Azure Resource Graph Query for Backup Status of Virtual Machines

## Overview

This document provides details on how to use the Kusto query to retrieve and analyze information about the backup status of Azure virtual machines and virtual machine scale sets. The query helps determine whether backups are configured and provides the last backup timestamp, if applicable.

## Usage

### Step 1: Access Azure Resource Graph Explorer

1. Log in to the [Azure Portal](https://portal.azure.com/).
2. Navigate to the **Resource Graph Explorer** by searching for "Resource Graph" in the top search bar.

### Step 2: Run the Query

1. Copy the Kusto query provided below.
2. Paste it into the query editor in the Resource Graph Explorer.
3. Click the **Run query** button to execute the query.

   ```kusto
   Resources
   | where type in~ ('microsoft.compute/virtualmachines', 'microsoft.classiccompute/virtualmachines', 'microsoft.compute/virtualmachinescalesets')
   | extend resourceId=tolower(id) 
   | join kind=leftouter (
       RecoveryServicesResources
       | where type == "microsoft.recoveryservices/vaults/backupfabrics/protectioncontainers/protecteditems"
       | where properties.backupManagementType == "AzureIaasVM"
       | project resourceId = tolower(tostring(properties.sourceResourceId)), backupItemid = id, 
                 lastBackupStatus = properties.lastBackupTime, 
                 isBackedUp = isnotempty(id)
   ) on resourceId 
   | extend isProtected = isnotempty(backupItemid)
   | extend status = iff(isProtected == bool(true), "Backup Configured", "Backup Not Configured")
   | extend lastBackup = iff(isProtected == bool(true), lastBackupStatus, "No backup configured")
   | project resourceId, isProtected, status, lastBackup

### Step 3: Download the Results

Once the query execution is complete, you will see the results displayed in a table format.  
To download the results, click on the **Download** button (downward arrow icon) located above the results table.  
Choose the desired format (CSV, JSON) and save the file to your local machine.


## Output

When you run the query, the output table will include the following fields:

- **resourceId**: The unique identifier for each virtual machine or virtual machine scale set, converted to lowercase for consistency.
- **isProtected**: Indicates whether a backup is configured for the resource. This is determined based on the presence of a backup item ID.
- **status**: Describes the backup configuration status of the resource. It will be "Backup Configured" if a backup is in place, otherwise "Backup Not Configured".
- **lastBackup**: The timestamp of the last backup if the resource is protected, or "No backup configured" if no backup is in place.

The query joins virtual machines and virtual machine scale sets with backup information to determine the backup status for each resource. It provides an overview of whether backups are configured and, if so, the time of the last backup.

## Conclusion

This Kusto query is a valuable tool for assessing the backup configuration of virtual machines and scale sets in your Azure environment. By running this query in the Azure Resource Graph Explorer, you can efficiently determine the backup status and last backup times for your resources, facilitating better backup management and compliance reporting.
