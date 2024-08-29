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
