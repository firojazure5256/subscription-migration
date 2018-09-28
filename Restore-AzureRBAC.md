# Restore-AzureRBAC
This script consumes all CLI XML exported with Backup-* scripts in order to re-create Azure AD Groups and Service Principals at the "new" tenant as well as re-applies the RBAC to subscriptions and fix Access Policies in Azure KeyVaults as well.

## Syntax
For Example, Powershell:
```powershell
.\Restore-AzureRBAC.ps1 -GroupExport <string> -UserExport <string> -ServicePrincipalExport <string> -SubscriptionExport <string> [-Prefix <string>] [-Verify] [-SkipKeyVault] [-SkipSubscription] [<CommonParameters>]
```

## Parameters
Table describing each of the parameters:

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| GroupExport | String | Yes | Path to the "AAD_Groups.xml" generated by Backup-AzureGroups.ps1 |
| UserExport | String | Yes | Path to the "AAD_Users.xml" generated by Backup-AzureUser.ps1 |
| ServicePrincipalExport | String | Yes | Path to the "AAD_ServicePrincipal.xml" generated by Backup-AzureServicePrincipal.ps1 |
| SubscriptionExport | String | Yes | Path to the "Subscriptions.xml" generated by Backup-AzureSubscriptions.ps1 |
| Prefix | String | No | Specifies a prefix to be added to re-created AAD Groups and Service Principal Names |
| Verify | switch | No | Switch used to make the script verify the XML have been properly exported |
| SkipKeyVault | switch | No | Switch to skip updating Access Policies in Azure KeyVaults |
| SkipSubscription | switch | No | Switch to skip updating RBAC in the Subscriptions |


## Example

### Example 1
Verify all exported XML are in the right format and are consumables

```powershell
.\Restore-AzureRBAC.ps1 -GroupExport ".\AAD_Groups.xml" -UserExport ".\AAD_Users.xml" -ServicePrincipalExport ".\AAD_ServicePrincipal.xml" -SubscriptionExport ".\Subscriptions.xml" -Verify
```

### Example 2
Re-create all missing AAD Groups and SPNs by adding "Migrated-" to their names, fix Azure KeyVault's access policies and re-apply subscription's RBAC.

```powershell
.\Restore-AzureRBAC.ps1 -GroupExport ".\AAD_Groups.xml" -UserExport ".\AAD_Users.xml" -ServicePrincipalExport ".\AAD_ServicePrincipal.xml" -SubscriptionExport ".\Subscriptions.xml" -Prefix "Migrated-"
```

### Example 3
Re-create all missing AAD Groups and SPNs with their same name as in the previous tenant and re-apply subscription's RBAC. Do not modify Azure KeyVaults

```powershell
.\Restore-AzureRBAC.ps1 -GroupExport ".\AAD_Groups.xml" -UserExport ".\AAD_Users.xml" -ServicePrincipalExport ".\AAD_ServicePrincipal.xml" -SubscriptionExport ".\Subscriptions.xml" -SkipKeyVault
```