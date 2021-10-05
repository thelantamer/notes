# git bash for windows

### ssh passphrase add
eval $(ssh-agent)
ssh-add c:\users\me\.ssh\id_rsa

### set subscription
az account set -s "subscription name here"

### validate template deployment
az group deployment validate --template-file ./template.json --parameters ./parameters.json -g my-resource-group --debug 

### new template deployment
az group deployment create --template-file ./template.json --parameters ./parameters.json -g my-resource-group --debug 

### ansible play
ansible-playbook -i localhost ./playfile.yaml

### app gateway list backend addresses
az network application-gateway show-backend-health -n gatewayname -g rsgname| grep address

### add route to route table
az network route-table route create -g resourcegroup --route-table-name vpnremotes -n rt_remotes_10_1_1_0__24 --next-hop-type VirtualAppliance --address-prefix 10.1.1.0/24 --next-hop-ip-address 10.170.250.36

# JMESPATH query examples

### show all dns servers
az network vnet list --query "[*].[name, dhcpOptions.dnsServers]"

### query for non-empty dns servers
az network vnet list --query "[?dhcpOptions.dnsServers != null].[name, dhcpOptions.dnsServers]"

### query vnets using 'contains' query | works in bash, not powershell
az network vnet list --query "[?contains(name,'product')].[name,resourceGroup]" -o tsv

### query sbnet using 'contains' query | works in bash, not powershell
az network vnet subnet list -g myresourcegroup --vnet-name myvnetname -o tsv --query "[?contains(name,'mailroom_outsourcing')].[id]"

### query list of vnets and address ranges with DNS servers
az network vnet list --query '[*].{VNET:name, SUBNET:addressSpace.addressPrefixes, DNS:dhcpOptions.dnsServers}'

### list running windows vms
 az vm list --show-details --query "[?storageProfile.osDisk.osType=='Windows' && powerState=='VM running'].{VMName:name, Power:powerState, RG:resourceGroup, State:provisioningState}" -o table

### query azure aservice principal for object id | works in powershell, not bash
az ad sp list --filter "displayname eq 'sp_environment_service_principal'" --query "[*].[displayName,objectId]"

### what-if deployment check
az deployment group what-if --resource-group MYRSG --template-file "c:\MYTEMPLATEFILE.json" --parameters "c:\MYPARAMETERSFILE\parameters.json"

### list vm ip addresses
az vm list-ip-addresses

# assign subnet permissions
az role assignment create --assignee 000-00-000-OBJECT-ID --role "Network Contributor" --scope /subscriptions/MY-SUBSCRIPTION-ID/resourceGroups/resource-group-name/providers/Microsoft.Network/virtualNetworks/my-virtual-machine

# check effective route from a VM
az network nic show-effective-route-table -n MYVM -g MYRSG -o table | Select-string -Pattern '192\.168\.75\.0'

# all but databricks and aks linux vms
az vm list --query "[? ! contains(resourceGroup,'DATABRICKS') && storageProfile.osDisk.osType=='Linux' && ! contains(name,'aks-agentpool')].[name,resourceGroup]" -o tsv