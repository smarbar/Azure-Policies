# Enforce DoNotDelete Lock on Resource Group

This policy adds the DoNotDelete lock to the resource group (which flows down to resources). It can be assigned at either the Management Group, Subscription or Resource Group level.

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsmarbar%2FAzure-Policies%2Fmain%2FLocks%2FEnforce-Do-Not-Delete%2Fazurepolicy.json)

## Try with Azure PowerShell

### Create Policy Definition

```powershell
# Create the Policy Definition (Management Group scope)
$definition = New-AzPolicyDefinition -Name 'enforce-do-not-delete-lock-on-resource-group' -DisplayName 'Deploy CanNotDelete Resource Lock on Resource Groups' -description 'Creates a resource lock at the resource group level for preventing resource deletion.' -metadata '{ "version": "1.0.0", "category": "General" }' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Locks/Enforce-Do-Not-Delete/azurepolicy.rules.json' -Mode All -ManagementGroupName 'YourManagementGroupName'
```

Or

```powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'enforce-do-not-delete-lock-on-resource-group' -DisplayName 'Deploy CanNotDelete Resource Lock on Resource Groups' -description 'Creates a resource lock at the resource group level for preventing resource deletion.' -metadata '{ "version": "1.0.0", "category": "General" }' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Locks/Enforce-Do-Not-Delete/azurepolicy.rules.json' -Mode All
```

### Assign the Policy definition

```powershell
# Set the scope to either Management Group, Subscription or Resource Group;
$scope = (Get-AzManagementGroup -Name 'YourResourceGroup').Id
$scope = (Get-AzSubscription -SubscriptionName 'YourResourceGroup').Id
$scope = (Get-AzResourceGroup -Name 'YourResourceGroup').ResourceId

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'enforce-do-not-delete-lock-on-resource-group' -DisplayName 'Deploy CanNotDelete Resource Lock on Resource Groups' -Scope $scope -PolicyDefinition $definition
```

## Try with Azure CLI

### Create Policy Definition

```cli
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'enforce-do-not-delete-lock-on-resource-group' --display-name 'Deploy CanNotDelete Resource Lock on Resource Groups' --description 'Creates a resource lock at the resource group level for preventing resource deletion.' --metadata '{ "version": "1.0.0", "category": "General" }' --rules 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Locks/Enforce-Do-Not-Delete/azurepolicy.rules.json' --mode All)
```

### Assign the Policy definition

```cli
# Set the scope to either Management Group, Subscription or Resource Group;
scope=$(az account management-group show --name 'YourManagementGroup')
scope=$(az account show --name 'YourSubscription')
scope=$(az group show --name 'YourResourceGroup')

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'enforce-do-not-delete-lock-on-resource-group' --display-name 'Deploy CanNotDelete Resource Lock on Resource Groups'  --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r`)
```
