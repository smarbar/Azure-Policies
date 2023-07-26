# Enforce Do Not Delete Lock on Resource Group

This policy adds the do DoNotDelete lock to the resource group (which flows down to resources).

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsmarbar%2FAzure-Policies%2Fmain%2FLocks%2FEnforce-Do-Not-Delete%2Fazurepolicy.json)

## Try with Azure PowerShell

```powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'enforce-do-not-delete-lock-on-resource-group' -DisplayName 'Deploy CanNotDelete Resource Lock on Resource Groups' -description 'Creates a resource lock at the resource group level for preventing resource deletion.' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Locks/Enforce-Do-Not-Delete/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Locks/Enforce-Do-Not-Delete/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'enforce-do-not-delete-lock-on-resource-group' -DisplayName 'Deploy CanNotDelete Resource Lock on Resource Groups' -Scope $scope.ResourceId -PolicyDefinition $definition
```

## Try with Azure CLI

```cli
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'enforce-do-not-delete-lock-on-resource-group' --display-name 'Deploy CanNotDelete Resource Lock on Resource Groups' --description 'Creates a resource lock at the resource group level for preventing resource deletion.' --rules 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Locks/Enforce-Do-Not-Delete/azurepolicy.rules.json' --mode All)

# Set the scope to a resource group; may also be a subscription or management group
scope=$(az group show --name 'YourResourceGroup')

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'enforce-do-not-delete-lock-on-resource-group' --display-name 'Deploy CanNotDelete Resource Lock on Resource Groups'  --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r`)
```
