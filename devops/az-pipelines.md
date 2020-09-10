az pipelines create --name vnet-products-vnets --folder-path rsg_vnet_products --description 'vnet-products-vnets' --repository rsg_vnet_products --branch master --yml-path vnets/src-yaml/azure_pipelines.yml --repository-type tfsgit

az pipelines create --name vnet-products-subnets --folder-path rsg_vnet_products --description 'vnet-products-subnets' --repository rsg_vnet_products --branch master --yml-path subnets/src-yaml/azure_pipelines.yml --repository-type tfsgit

az pipelines create --name vnet-products-vnet-peering --folder-path rsg_vnet_products --description 'vnet-products-vnet-peering' --repository rsg_vnet_products --branch master --yml-path vnet-peering/src-yaml/azure_pipelines.yml --repository-type tfsgit