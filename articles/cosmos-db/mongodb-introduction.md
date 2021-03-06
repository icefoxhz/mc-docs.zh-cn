---
title: Azure Cosmos DB 的用于 MongoDB 的 API 简介
description: 了解如何使用 Azure Cosmos DB，通过 Azure Cosmos DB 的用于 MongoDB 的 API 来存储和查询大量数据。
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
origin.date: 10/01/2019
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: ''
author: rockboyfor
ms.author: v-yeche
ms.openlocfilehash: 3893743eee1c524618c20d7aa2f9f39a9dda6649
ms.sourcegitcommit: b9dfda0e754bc5c591e10fc560fe457fba202778
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91246398"
---
# <a name="azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB 的用于 MongoDB 的 API

[Azure Cosmos DB](introduction.md) 是世纪互联针对任务关键型应用程序提供的多区域分配式多模型数据库服务。 Azure Cosmos DB 在中国各地提供[统包式多区域分发](distribute-data-globally.md)、[吞吐量和存储的弹性扩展](partition-data.md)、99% 的情况下低至个位数的毫秒级延迟以及得到保证的高可用性，所有这些均由[行业领先的 SLA](https://www.azure.cn/support/sla/cosmos-db/) 提供支持。 Azure Cosmos DB [自动为数据编制索引](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf)，无需客户管理架构和索引。 它是多模型的，支持文档、键值、图和列式数据模型。Azure Cosmos DB 服务为包含 Cassandra、MongoDB、Gremlin 和 Azure 表存储在内的常见 NoSQL API 实现 Wire Protocol。 这样，你便可以使用熟悉的 NoSQL 客户端驱动程序和工具来与 Cosmos 数据库交互。

## <a name="wire-protocol-compatibility"></a>网络协议兼容性

Azure Cosmos DB 实现 MongoDB 的 Wire Protocol。 此实现允许与本机 MongoDB 客户端 SDK、驱动程序和工具进行透明兼容。 Azure Cosmos DB 托管 MongoDB 数据库引擎。 可在此处找到受 MongoDB 支持的功能的详细信息： 
- [Azure Cosmos DB API for Mongo DB 引擎 3.6 版](mongodb-feature-support-36.md)
- [Azure Cosmos DB API for Mongo DB 引擎 3.2 版](mongodb-feature-support.md)

默认情况下，使用 Azure Cosmos DB 的 API for MongoDB 创建的新帐户与 MongoDB 线路协议 3.6 版兼容。 任何识别此协议版本的 MongoDB 客户端驱动程序应该可以本机连接到 Cosmos DB。

:::image type="content" source="./media/mongodb-introduction/cosmosdb-mongodb.png" alt-text="Azure Cosmos DB 的 API for MongoDB" border="false":::

## <a name="key-benefits"></a>主要优点

作为一种完全托管的多区域分布式数据库即服务，Cosmos DB 的主要优势详见[此处](introduction.md)。 此外，Cosmos DB 通过本机实现流行 NoSQL API 的网络协议来提供以下优势：

* 轻松将应用程序迁移到 Cosmos DB，同时保留应用程序逻辑的重要部分。
* 使应用程序保持可移植性，并继续保持云供应商的不可知性。
* 为 Cosmos DB 支持的常用 NoSQL API 获取行业领先的、有资金保障的 SLA。
* 根据需求弹性缩放 Cosmos 数据库的预配吞吐量和存储，并且只需为使用的吞吐量和存储付费。 这可以大幅节省成本。
* 通过多主数据库复制功能实现统包式多区域分布。

## <a name="cosmos-dbs-api-for-mongodb"></a>Cosmos DB 的用于 MongoDB 的 API

遵循快速入门创建 Azure Cosmos 帐户，并迁移现有 MongoDB 应用程序以使用 Azure Cosmos DB，或者生成一个新的应用程序：

* [迁移现有的 MongoDB Node.js Web 应用](create-mongodb-nodejs.md)。
* [使用 Azure Cosmos DB 的用于 MongoDB 的 API 和 .NET SDK 生成 Web 应用](create-mongodb-dotnet.md)
* [使用 Azure Cosmos DB 的用于 MongoDB 的 API 和 Java SDK 生成控制台应用](create-mongodb-java.md)

## <a name="next-steps"></a>后续步骤

下面是一些可帮助你入门的指南：

* 在[将 MongoDB 应用程序连接到 Azure Cosmos DB](connect-mongodb-account.md) 教程中了解如何获取帐户连接字符串信息。
* 在[将 Studio 3T 与 Azure Cosmos DB 配合使用](mongodb-mongochef.md)教程中了解如何在 Studio 3T 中创建 Cosmos 数据库与 MongoDB 应用之间的连接。
* 在[将 MongoDB 数据导入 Azure Cosmos DB](mongodb-migrate.md) 教程中了解如何将数据导入 Cosmos 数据库。
* 使用 [Robo 3T](mongodb-robomongo.md) 连接到 Cosmos 帐户。
* 了解如何[配置多区域分布式应用的读取首选项](../cosmos-db/tutorial-global-distribution-mongodb.md)。
* 在[故障排除指南](mongodb-troubleshoot.md)中查找常见错误的解决方案

<sup>注意：本文介绍了可与 MongoDB 数据库实现线路协议兼容的 Azure Cosmos DB 功能。Azure 不会运行 MongoDB 数据库来提供此服务。Azure Cosmos DB 并不隶属于 MongoDB, inc.</sup>

<!-- Update_Description: update meta properties, wording update, update link -->