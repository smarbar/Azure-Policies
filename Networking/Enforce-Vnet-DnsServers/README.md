# Enforce DNS Servers on VNET

This policy enforces DNS servers on VNETS.

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsmarbar%2FAzure-Policies%2Fmain%2FNetworking%2FEnforce-Vnet-DnsServers%2Fazurepolicy.json)

## Try with Azure PowerShell

### Create Policy Definition

```powershell
# Create the Policy Definition (Management Group scope)
$definition = New-AzPolicyDefinition -Name 'enforce-dns-servers-on-vnets' -DisplayName 'Enforce VNET DNS servers' -description 'This policy prevent setting non authorized dns servers for vnets.' -metadata '{ "version": "1.0.0", "category": "Network" }' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Enforce-Vnet-DnsServers/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Enforce-Vnet-DnsServers/azurepolicy.parameters.json' -Mode All -ManagementGroupName 'YourManagementGroupName'
```

Or

```powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'enforce-dns-servers-on-vnets' -DisplayName 'Enforce VNET DNS servers' -description 'This policy prevent setting non authorized dns servers for vnets.' -metadata '{ "version": "1.0.0", "category": "Network" }' -Policy 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Enforce-Vnet-DnsServers/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Enforce-Vnet-DnsServers/azurepolicy.parameters.json' -Mode All
```

### Assign the Policy definition

```powershell
# Set the scope to either Management Group, Subscription or Resource Group;
$scope = (Get-AzManagementGroup -Name 'YourResourceGroup').Id
$scope = (Get-AzSubscription -SubscriptionName 'YourResourceGroup').Id
$scope = (Get-AzResourceGroup -Name 'YourResourceGroup').ResourceId

# Set the Policy Parameter (JSON format)
$policyparam = '{ "servers":  [ "DnsServerIP1", "DnsServerIP2" ], "effect": "Audit" }'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'enforce-dns-servers-on-vnets' -DisplayName 'Enforce VNET DNS servers' -Scope $scope -PolicyDefinition $definition -PolicyParameter $policyparam
```

## Try with Azure CLI

### Create Policy Definition

```cli
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --Name 'enforce-dns-servers-on-vnets' --display-name 'Enforce VNET DNS servers' --description 'This policy prevent setting non authorized dns servers for vnets.' --metadata '{ "version": "1.0.0", "category": "Network" }' --rules 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Enforce-Vnet-DnsServers/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/smarbar/Azure-Policies/main/Networking/Enforce-Vnet-DnsServers/azurepolicy.parameters.json' --mode All)
```

### Assign the Policy definition

```cli
# Set the scope to either Management Group, Subscription or Resource Group;
scope=$(az account management-group show --name 'YourManagementGroup')
scope=$(az account show --name 'YourSubscription')
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyparam='{ "servers":  [ "DnsServerIP1", "DnsServerIP2" ], "effect: "Audit" }'

# Create the Policy Assignment
assignment=$(az policy assignment create --Name 'enforce-dns-servers-on-vnets' --display-name 'Enforce VNET DNS servers'  --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
```
