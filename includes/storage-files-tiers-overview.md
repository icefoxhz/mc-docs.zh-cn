---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 12/27/2019
ms.date: 09/28/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: d3cf2d19ef9dd68c061d681b7ee7283ea74d8c5c
ms.sourcegitcommit: 119a3fc5ffa4768b1bd8202191091bd4d873efb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91026529"
---
Azure 文件存储提供了四种不同的存储层（高级、事务优化、热和冷存储层），因此你能够根据方案的性能和价格要求定制共享：

- **Premium**：高级文件共享由固态硬盘 (SSD) 支持，并部署在 FileStorage 存储帐户类型中。 高级文件共享提供稳定的高性能和低延迟，对于大多数 IO 操作和 IO 密集型工作负荷，延迟不到 10 毫秒。 这使得它们适合于各种各样的工作负荷，例如数据库、网站托管和开发环境。 
- **事务优化**：事务优化文件共享可实现事务繁重的工作负载，这些工作负荷不需要高级文件共享提供的延迟。 事务优化文件共享在硬盘驱动器 (HDD) 支持的标准存储硬件上提供，并部署在常规用途版本 2 (GPv2) 存储帐户类型中。 虽然此存储层一直被称为“标准”层，但这指的是存储介质类型而不是层本身（热和冷存储层也是“标准”层，因为它们位于标准存储硬件上）。
- **Hot**：热文件共享提供针对常规用途文件共享方案（如团队共享）优化的存储。 热文件共享在 HDD 支持的标准存储硬件上提供，并部署在常规用途版本 2 (GPv2) 存储帐户类型中。
- **Cool**：冷文件共享提供针对在线存档存储方案优化的经济高效的存储。 冷文件共享在 HDD 支持的标准存储硬件上提供，并部署在常规用途版本 2 (GPv2) 存储帐户类型中。

高级文件共享只能在预配的计费模型下使用。 有关高级文件共享的预配计费模型的详细信息，请参阅[了解预配高级文件共享](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares)。 标准文件共享（包括事务优化共享、热文件共享和冷文件共享）以预付费模式提供。

若要部署热或冷文件共享，请参阅[创建热或冷文件共享](../articles/storage/files/storage-how-to-create-file-share.md#create-a-hot-or-cool-file-share)。 