---
title: Azure Virtual Network
description: Learn about Azure Virtual Network concepts and features, including address space, subnets, regions, and subscriptions. 
services: virtual-network
documentationcenter: na
author: KumudD
# Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/03/2020
ms.author: kumud
---

# What is Azure Virtual Network?

Azure Virtual Network (VNet) is the fundamental building block for your private network in Azure. VNet enables many types of Azure resources, such as Azure Virtual Machines (VM), to securely communicate with each other, the internet, and on-premises networks. VNet is similar to a traditional network that you'd operate in your own data center, but brings with it additional benefits of Azure's infrastructure such as scale, availability, and isolation.

## Why use an Azure Virtual network?
Azure virtual network enables Azure resources to securely communicate with each other, the internet, and on-premises networks. Key scenarios that you can accomplish with a virtual network include - communication of Azure resources with the internet, communication between Azure resources, communication with on-premises resources, filtering network traffic, routing network traffic, and integration with Azure services.

### Communicate with the internet

All resources in a VNet can communicate outbound to the internet, by default. You can communicate inbound to a resource by assigning a public IP address or a public Load Balancer. You can also use public IP or public Load Balancer to manage your outbound connections.  To learn more about outbound connections in Azure, see [Outbound connections](../load-balancer/load-balancer-outbound-connections.md), [Public IP addresses](./ip-services/virtual-network-public-ip-address.md), and [Load Balancer](../load-balancer/load-balancer-overview.md).

>[!NOTE]
>When using only an internal [Standard Load Balancer](../load-balancer/load-balancer-overview.md), outbound connectivity is not available until you define how you want [outbound connections](../load-balancer/load-balancer-outbound-connections.md) to work with an instance-level public IP or a public Load Balancer.

### Communicate between Azure resources

Azure resources communicate securely with each other in one of the following ways:

- **Through a virtual network**: You can deploy VMs, and several other types of Azure resources to a virtual network, such as Azure App Service Environments, the Azure Kubernetes Service (AKS), and Azure Virtual Machine Scale Sets. To view a complete list of Azure resources that you can deploy into a virtual network, see [Virtual network service integration](virtual-network-for-azure-services.md).
- **Through a virtual network service endpoint**: Extend your virtual network private address space and the identity of your virtual network to Azure service resources, such as Azure Storage accounts and Azure SQL Database, over a direct connection. Service endpoints allow you to secure your critical Azure service resources to only a virtual network. To learn more, see [Virtual network service endpoints overview](virtual-network-service-endpoints-overview.md).
- **Through VNet Peering**: You can connect virtual networks to each other, enabling resources in either virtual network to communicate with each other, using virtual network peering. The virtual networks you connect can be in the same, or different, Azure regions. To learn more, see [Virtual network peering](virtual-network-peering-overview.md).

### Communicate with on-premises resources

You can connect your on-premises computers and networks to a virtual network using any combination of the following options:

- **Point-to-site virtual private network (VPN):** Established between a virtual network and a single computer in your network. Each computer that wants to establish connectivity with a virtual network must configure its connection. This connection type is great if you're just getting started with Azure, or for developers, because it requires little or no changes to your existing network. The communication between your computer and a virtual network is sent through an encrypted tunnel over the internet. To learn more, see [Point-to-site VPN](../vpn-gateway/point-to-site-about.md?toc=%2fazure%2fvirtual-network%2ftoc.json#).
- **Site-to-site VPN:** Established between your on-premises VPN device and an Azure VPN Gateway that is deployed in a virtual network. This connection type enables any on-premises resource that you authorize to access a virtual network. The communication between your on-premises VPN device and an Azure VPN gateway is sent through an encrypted tunnel over the internet. To learn more, see [Site-to-site VPN](../vpn-gateway/design.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti).
- **Azure ExpressRoute:** Established between your network and Azure, through an ExpressRoute partner. This connection is private. Traffic does not go over the internet. To learn more, see [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### Filter network traffic

You can filter network traffic between subnets using either or both of the following options:

- **Network security groups:** Network security groups and application security groups can contain multiple inbound and outbound security rules that enable you to filter traffic to and from resources by source and destination IP address, port, and protocol. To learn more, see [Network security groups](./network-security-groups-overview.md#network-security-groups) or [Application security groups](./network-security-groups-overview.md#application-security-groups).
- **Network virtual appliances:** A network virtual appliance is a VM that performs a network function, such as a firewall, WAN optimization, or other network function. To view a list of available network virtual appliances that you can deploy in a virtual network, see [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

### Route network traffic

Azure routes traffic between subnets, connected virtual networks, on-premises networks, and the Internet, by default. You can implement either or both of the following options to override the default routes Azure creates:

- **Route tables:** You can create custom route tables with routes that control where traffic is routed to for each subnet. Learn more about [route tables](virtual-networks-udr-overview.md#user-defined).
- **Border gateway protocol (BGP) routes:** If you connect your virtual network to your on-premises network using an Azure VPN Gateway or ExpressRoute connection, you can propagate your on-premises BGP routes to your virtual networks. Learn more about using BGP with [Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) and [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange).

### Virtual network integration for Azure services

Integrating Azure services to an Azure virtual network enables private access to the service from virtual machines or compute resources in the virtual network.
You can integrate Azure services in your virtual network with the following options:
- Deploying [dedicated instances of the service](virtual-network-for-azure-services.md) into a virtual network. The services can then be privately accessed within the virtual network and from on-premises networks.
- Using [Private Link](../private-link/private-link-overview.md) to access privately a specific instance of the service from your virtual network and from on-premises networks.
- You can also access the service using public endpoints by extending a virtual network to the service, through [service endpoints](virtual-network-service-endpoints-overview.md). Service endpoints allow service resources to be secured to the virtual network.
 

## Azure VNet limits

There are certain limits around the number of Azure resources you can deploy. Most Azure networking limits are at the maximum values. However, you can [increase certain networking limits](../azure-portal/supportability/networking-quota-requests.md) as specified on the [VNet limits page](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits). 

## Virtual networks and availability zones
Virtual networks and subnets span all availability zones in a region. You don't need to divide them by availability zones to accommodate zonal resources. For example, if you configure a zonal VM, you don't have to take into consideration the virtual network when selecting the availability zone for the VM. The same is true for other zonal resources.

## Pricing

There is no charge for using Azure VNet, it is free of cost. Standard charges are applicable for resources, such as Virtual Machines (VMs) and other products. To learn more, see [VNet pricing](https://azure.microsoft.com/pricing/details/virtual-network/) and the Azure [pricing calculator](https://azure.microsoft.com/pricing/calculator/).

## Next steps
 - Learn about [Azure Virtual Network concepts and best practices](concepts-and-best-practices.md) .
 - To get started using a virtual network, create one, deploy a few VMs to it, and communicate between the VMs. To learn how, see the [Create a virtual network](quick-create-portal.md) quickstart.