# add ssh passphrase
## git bash for windows
eval $(ssh-agent)
ssh-add c:\users\me\.ssh\id_rsa

# set subscription
az account set -s "subscription name here"

# validate template deployment
az group deployment validate --template-file ./template.json --parameters ./parameters.json -g my-resource-group --debug 

# new template deployment
az group deployment create --template-file ./template.json --parameters ./parameters.json -g my-resource-group --debug 

# ansible play
ansible-playbook -i localhost ./playfile.yaml

# app gateway list backend addresses
az network application-gateway show-backend-health -n gatewayname -g rsgname| grep address

# add route to route table
az network route-table route create -g resourcegroup --route-table-name vpnremotes -n rt_remotes_10_1_1_0__24 --next-hop-type VirtualAppliance --address-prefix 10.1.1.0/24 --next-hop-ip-address 10.170.250.36

// JMESPATH query examples

# show all dns servers
az network vnet list --query "[*].[name, dhcpOptions.dnsServers]"

# query for non-empty dns servers
az network vnet list --query "[?dhcpOptions.dnsServers != null].[name, dhcpOptions.dnsServers]"

// what-if deployment check

az deployment group what-if --resource-group MYRSG --template-file "c:\MYTEMPLATEFILE.json" --parameters "c:\MYPARAMETERSFILE\parameters.json"

# query list of vnets and address ranges with DNS servers
z network vnet list --query '[*].{VNET:name, SUBNET:addressSpace.addressPrefixes, DNS:dhcpOptions.dnsServers}'

# assign subnet permissions
az role assignment create --assignee 000-00-000-OBJECT-ID --role "Network Contributor" --scope /subscriptions/MY-SUBSCRIPTION-ID/resourceGroups/resource-group-name/providers/Microsoft.Network/virtualNetworks/my-virtual-machine