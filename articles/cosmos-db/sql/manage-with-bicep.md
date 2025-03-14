---
title: Create and manage Azure Cosmos DB with Bicep
description: Use Bicep to create and configure Azure Cosmos DB for Core (SQL) API 
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: how-to
ms.date: 09/13/2021
ms.author: mjbrown
---

# Manage Azure Cosmos DB Core (SQL) API resources with Bicep

[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

In this article, you learn how to use Bicep to deploy and manage your Azure Cosmos DB accounts, databases, and containers.

This article shows Bicep samples for Core (SQL) API accounts. You can also find Bicep samples for [Cassandra](../cassandra/manage-with-bicep.md), [Gremlin](../graph/manage-with-bicep.md), [MongoDB](../mongodb/manage-with-bicep.md), and [Table](../table/manage-with-bicep.md) APIs.

> [!IMPORTANT]
>
> * Account names are limited to 44 characters, all lowercase.
> * To change the throughput values, redeploy the Bicep file with updated RU/s.
> * When you add or remove locations to an Azure Cosmos account, you can't simultaneously modify other properties. These operations must be done separately.
> * Azure Cosmos DB resources cannot be renamed as this violates how Azure Resource Manager works with resource URIs.
> * To provision throughput at the database level and share across all containers, apply the throughput values to the database options property.

To create any of the Azure Cosmos DB resources below, copy the following example into a new bicep file. You can optionally create a parameters file to use when deploying multiple instances of the same resource with different names and values. There are many ways to deploy Azure Resource Manager templates including, [Azure CLI](../../azure-resource-manager/bicep/deploy-cli.md), [Azure PowerShell](../../azure-resource-manager/bicep/deploy-powershell.md) and [Cloud Shell](../../azure-resource-manager/bicep/deploy-cloud-shell.md).

<a id="create-autoscale"></a>

## Azure Cosmos account with autoscale throughput

Create an Azure Cosmos account in two regions with options for consistency and failover, with database and container configured for autoscale throughput that has most policy options enabled.

:::code language="bicep" source="~/quickstart-templates/quickstarts/microsoft.documentdb/cosmosdb-sql-autoscale/main.bicep":::

<a id="create-analytical-store"></a>

## Azure Cosmos account with analytical store

Create an Azure Cosmos account in one region with a container with Analytical TTL enabled and options for manual or autoscale throughput.

:::code language="bicep" source="~/quickstart-templates/quickstarts/microsoft.documentdb/cosmosdb-sql-analytical-store/main.bicep":::

<a id="create-manual"></a>

## Azure Cosmos account with standard provisioned throughput

Create an Azure Cosmos account in two regions with options for consistency and failover, with database and container configured for standard throughput that has most policy options enabled.

:::code language="bicep" source="~/quickstart-templates/quickstarts/microsoft.documentdb/cosmosdb-sql/main.bicep":::

<a id="create-sproc"></a>

## Azure Cosmos DB container with server-side functionality

Create an Azure Cosmos account, database and container with with a stored procedure, trigger, and user-defined function.

:::code language="bicep" source="~/quickstart-templates/quickstarts/microsoft.documentdb/cosmosdb-sql-container-sprocs/main.bicep":::

<a id="create-rbac"></a>

## Azure Cosmos DB account with Azure AD and RBAC

Create an Azure Cosmos account, a natively maintained Role Definition, and a natively maintained Role Assignment for an AAD identity.

:::code language="bicep" source="~/quickstart-templates/quickstarts/microsoft.documentdb/cosmosdb-sql-rbac/main.bicep":::

<a id="free-tier"></a>

## Free tier Azure Cosmos DB account

Create a free-tier Azure Cosmos account and a database with shared throughput that can be shared with up to 25 containers.

:::code language="bicep" source="~/quickstart-templates/quickstarts/microsoft.documentdb/cosmosdb-free/main.bicep":::

## Next steps

Here are some additional resources:

* [Bicep documentation](../../azure-resource-manager/bicep/index.yml)
* [Install Bicep tools](../../azure-resource-manager/bicep/install.md)
