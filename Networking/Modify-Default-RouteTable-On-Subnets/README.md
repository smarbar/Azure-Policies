# Modify a Default RouteTable on Subnets

This policy adds or replaces the RouteTable assigned to subnets.

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsmarbar%2FAzure-Policies%2Fmain%2FNetworking%2FModify-Default-RouteTable-On-Subnets%2Fazurepolicy.json)

## Try with Azure PowerShell

### Create Policy Definition

```powershell
# Create the Policy Definition (Management Group scope)
$definition = New-AzPolicyDefinition -Name 'modify-default-route-table-on-subnets' -DisplayName 'Modify a default Route Table on subnets' -description 'Adds a route table to subnets. Other route tables are replaced with the default route table.' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Modify-Default-RouteTable-On-Subnets/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Modify-Default-RouteTable-On-Subnets/azurepolicy.parameters.json' -Mode All -ManagementGroupName 'YourManagementGroupName'
```

Or

```powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'modify-default-route-table-on-subnets' -DisplayName 'Modify a default Route Table on subnets' -description 'Adds a route table to subnets. Other route tables are replaced with the default route table.' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Modify-Default-RouteTable-On-Subnets/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Modify-Default-RouteTable-On-Subnets/azurepolicy.parameters.json' -Mode All
```

### Assign the Policy definition

```powershell
# Set the scope to either Management Group, Subscription or Resource Group;
$scope = (Get-AzManagementGroup -Name 'YourResourceGroup').Id
$scope = (Get-AzSubscription -SubscriptionName 'YourResourceGroup').Id
$scope = (Get-AzResourceGroup -Name 'YourResourceGroup').ResourceId


$routeTable = Get-AzRouteTable -Name 'YourRouteTableName'

# Set the Policy Parameter (JSON format)
$policyparam = "{ `"routeTableSettings`": { `"value`": ${$routeTable} } }" <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'modify-default-route-table-on-subnets' -DisplayName 'Modify a default Route Table on subnets' -Scope $scope -PolicyDefinition $definition -PolicyParameter $policyparam
```

## Try with Azure CLI

### Create Policy Definition

```cli
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --Name 'modify-default-route-table-on-subnets' --display-name 'Modify a default Route Table on subnets' --description 'Adds a route table to subnets. Other route tables are replaced with the default route table.' --rules 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Modify-Default-RouteTable-On-Subnets/azurepolicy.rules.json' --mode All)
```

### Assign the Policy definition

```cli
# Set the scope to either Management Group, Subscription or Resource Group;
scope=$(az account management-group show --name 'YourManagementGroup')
scope=$(az account show --name 'YourSubscription')
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyparam='{ "imageIds": { "value": [ "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage", "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510" ] } }'

# Create the Policy Assignment
assignment=$(az policy assignment create --Name 'modify-default-route-table-on-subnets' --display-name 'Modify a default Route Table on subnets'  --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
```
