# Require a Tag on Resource Group with exclusions

This policy will deny a resource group creation unless a specific tag has also been added, except where the resource group name matches one in a list of exclusions.

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsmarbar%2FAzure-Policies%2Fmain%2FTagging%2FRequire-tag-on-resource-groups-with-name-exclusions%2Fazurepolicy.json)

## Try with Azure PowerShell

### Create Policy Definition

```powershell
# Create the Policy Definition (Management Group scope)
$definition = New-AzPolicyDefinition -Name 'require-tag-on-resource-group-with-name-exclusions' -DisplayName 'Require a tag on resource groups with name exclusions' -description 'Enforces existence of a tag on resource groups. With name pattern exclusions' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Tagging/Require-tag-on-resource-groups-with-name-exclusions/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Tagging/Require-tag-on-resource-groups-with-name-exclusions/azurepolicy.parameters.json' -Mode All -ManagementGroupName 'YourManagementGroupName'
```

Or

```powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'require-tag-on-resource-group-with-name-exclusions' -DisplayName 'Require a tag on resource groups with name exclusions' -description 'Enforces existence of a tag on resource groups. With name pattern exclusions' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Tagging/Require-tag-on-resource-groups-with-name-exclusions/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Tagging/Require-tag-on-resource-groups-with-name-exclusions/azurepolicy.parameters.json' -Mode All
```

### Assign the Policy definition

```powershell
# Set the scope to either Management Group, Subscription or Resource Group;
$scope = (Get-AzManagementGroup -Name 'YourResourceGroup').Id
$scope = (Get-AzSubscription -SubscriptionName 'YourResourceGroup').Id
$scope = (Get-AzResourceGroup -Name 'YourResourceGroup').ResourceId


$routeTable = Get-AzRouteTable -Name 'YourRouteTableName'

# Set the Policy Parameter (JSON format)
$policyparam = "{ `"routeTableSettings`": { `"value`": ${$routeTable} } }"

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'require-tag-on-resource-group-with-name-exclusions' -DisplayName 'Require a tag on resource groups with name exclusions' -Scope $scope -PolicyDefinition $definition -PolicyParameter $policyparam
```

## Try with Azure CLI

### Create Policy Definition

```cli
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --Name 'require-tag-on-resource-group-with-name-exclusions' --display-name 'Require a tag on resource groups with name exclusions' --description 'Enforces existence of a tag on resource groups. With name pattern exclusions' --rules 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Tagging/Require-tag-on-resource-groups-with-name-exclusions/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Tagging/Require-tag-on-resource-groups-with-name-exclusions/azurepolicy.parameters.json' --mode All)
```

### Assign the Policy definition

```cli
# Set the scope to either Management Group, Subscription or Resource Group;
scope=$(az account management-group show --name 'YourManagementGroup')
scope=$(az account show --name 'YourSubscription')
scope=$(az group show --name 'YourResourceGroup')

# Create the Policy Assignment
assignment=$(az policy assignment create --Name 'require-tag-on-resource-group-with-name-exclusions' --display-name 'Require a tag on resource groups with name exclusions'  --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r`)
```
