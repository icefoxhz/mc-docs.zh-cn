---
title: 指标顾问服务加密
titleSuffix: Azure Cognitive Services
description: 指标顾问服务静态数据加密。
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: v-johya
ms.openlocfilehash: 8d4a8ed2e9c64ad9221251ed3852fcf86c858889
ms.sourcegitcommit: 6b499ff4361491965d02bd8bf8dde9c87c54a9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94329023"
---
# <a name="metrics-advisor-service-encryption-of-data-at-rest"></a>指标顾问服务静态数据加密

指标顾问服务在将数据保存到云时会自动加密数据。 指标顾问服务加密可保护数据，并帮助组织履行在安全性与合规性方面做出的承诺。

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> 客户管理的密钥仅在 E0 定价层上可用。 若要请求使用客户管理的密钥的能力，请填写并提交[指标顾问服务客户管理的密钥请求表单](https://aka.ms/cogsvc-cmk)。 你大约需要 3-5 个工作日才能收到关于请求状态的回复。 视情况而定，你可能需要排队，并在空间可用时获批。 获批可在指标顾问服务中使用 CMK 后，需要新建指标顾问资源并选择 E0 作为定价层。 创建具有 E0 定价层的指标顾问资源后，可使用 Azure Key Vault 设置托管标识。

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>后续步骤

* [指标顾问服务客户管理的密钥请求表单](https://aka.ms/cogsvc-cmk)
* [详细了解 Azure 密钥保管库](../../key-vault/general/overview.md)