az pipelines create --name vnet-products-vnets --folder-path rsg_vnet_products --description 'vnet-products-vnets' --repository rsg_vnet_products --branch master --yml-path /vnets/src-yaml/azure_pipelines.yml --repository-type tfsgit

    # deploy devops pipeline using branch development
    az pipelines run --name expressroute --branch development --open