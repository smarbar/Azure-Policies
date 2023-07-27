# Audit Subnets without a Route Table assigned

This policy audits subnets that do not have a Route Table assigned

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsmarbar%2FAzure-Policies%2Fmain%2FNetworking%2FAudit-Subnets-Without-RouteTable%2Fazurepolicy.json)

## Try with Azure PowerShell

### Create Policy Definition

```powershell
# Create the Policy Definition (Management Group scope)
$definition = New-AzPolicyDefinition -Name 'audit-route-table-on-vnet-subnets' -DisplayName 'Audit Route Table on subnets in vnets' -description 'Custom policy to audit if a route table is assigned to subnets in all vnets' -metadata '{ "version": "1.0.0", "category": "Network" }' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Audit-Subnets-Without-RouteTable/azurepolicy.rules.json' -Mode All -ManagementGroupName 'YourManagementGroupName'
```

Or

```powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'audit-route-table-on-vnet-subnets' -DisplayName 'Audit Route Table on subnets in vnets' -description 'Custom policy to audit if a route table is assigned to subnets in all vnets' -metadata '{ "version": "1.0.0", "category": "Network" }' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Audit-Subnets-Without-RouteTable/azurepolicy.rules.json' -Mode All
```

### Assign the Policy definition

```powershell
# Set the scope to either Management Group, Subscription or Resource Group;
$scope = (Get-AzManagementGroup -Name 'YourResourceGroup').Id
$scope = (Get-AzSubscription -SubscriptionName 'YourResourceGroup').Id
$scope = (Get-AzResourceGroup -Name 'YourResourceGroup').ResourceId

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'audit-route-table-on-vnet-subnets' -DisplayName 'Audit Route Table on subnets in vnets' -Scope $scope -PolicyDefinition $definition
```

## Try with Azure CLI

### Create Policy Definition

```cli
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --Name 'audit-route-table-on-vnet-subnets' --display-name 'Audit Route Table on subnets in vnets' --description 'Custom policy to audit if a route table is assigned to subnets in all vnets' --metadata '{ "version": "1.0.0", "category": "Network" }' --rules 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Audit-Subnets-Without-RouteTable/azurepolicy.rules.json' --mode All)
```

### Assign the Policy definition

```cli
# Set the scope to either Management Group, Subscription or Resource Group;
scope=$(az account management-group show --name 'YourManagementGroup')
scope=$(az account show --name 'YourSubscription')
scope=$(az group show --name 'YourResourceGroup')

# Create the Policy Assignment
assignment=$(az policy assignment create --Name 'audit-route-table-on-vnet-subnets' --display-name 'Audit Route Table on subnets in vnets'  --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r`)
```
