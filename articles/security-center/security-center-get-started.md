---
title: 升级到标准层 - Azure 安全中心
description: 本快速入门介绍如何升级到安全中心的标准定价层，以提高安全性。
services: security-center
documentationcenter: na
author: Johnnytechn
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 12/3/2018
ms.date: 09/14/2020
ms.author: v-johya
ms.openlocfilehash: 43e23ea221a0f2899bfb94fec8723629f7988e43
ms.sourcegitcommit: 41e986cd4a2879d8767dc6fc815c805e782dc7e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2020
ms.locfileid: "90822375"
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>快速入门：将 Azure 订阅加入安全中心标准层
Azure 安全中心跨混合云工作负荷提供统一的安全管理和威胁防护。 免费层只能为 Azure 资源提供有限的安全性，而标准层将这些功能扩展到了本地和其他云。 借助安全中心标准层，可以查找和修复安全漏洞、应用访问控制和应用程序控制来阻止恶意活动、使用分析和智能功能检测威胁，以及在受到攻击时迅速做出响应。 可以免费试用安全中心标准版。 若要了解详细信息，请参阅[定价页](https://www.azure.cn/pricing/details/security-center/)。

本文介绍如何升级到标准层以提高安全性，以及如何在虚拟机上安装 Log Analytics 代理来监视安全漏洞和威胁。

## <a name="prerequisites"></a>先决条件
若要开始使用安全中心，必须订阅 Azure。 如果你没有订阅，则可以注册[试用版](https://www.azure.cn/pricing/1rmb-trial/)。

若要将订阅升级到标准层，必须拥有“订阅所有者”、“订阅参与者”或“安全管理员”角色。

## <a name="enable-your-azure-subscription"></a>启用 Azure 订阅

1. 登录到 [Azure 门户](https://portal.azure.cn/)。

1. 在“Azure”菜单上选择“安全中心”。 此时会打开“安全中心 - 概览”。

   ![安全中心概述][2]

“安全中心 - 概述”提供了统一的视图，用于查看混合云工作负荷的安全态势，可让你发现和评估工作负荷的安全性，以及识别和缓解风险。 安全中心会自动启用以前尚未由你或其他订阅用户加入到免费层的所有 Azure 订阅。

可以通过单击“订阅”菜单项来查看和筛选订阅列表。 现在，安全中心将开始评估这些订阅的安全性，以识别安全漏洞。 若要自定义评估类型，可以修改安全策略。 安全策略定义了工作负载的相应配置，有助于确保用户遵守公司或法规方面的安全要求。

在首次启动安全中心后的几分钟内，可以看到：

- 有关如何改善 Azure 订阅安全性的**建议**。 单击“建议”磁贴会启动一个优先级列表。
- 该列表中包含安全中心目前正在评估的“计算和应用”、“网络”、“数据安全性”以及“标识和访问”资源的清单以及每个项的安全局势。

若要充分利用安全中心，需要按以下步骤升级到标准层，并安装 Log Analytics 代理。


## <a name="upgrade-to-the-standard-tier"></a>升级到标准层

若要学习安全中心快速入门和教程，必须升级到标准层。 有一个免费试用的安全中心标准版。 若要了解详细信息，请参阅[定价页](https://www.azure.cn/pricing/details/security-center/)。 

1. 从安全中心的边栏选择“开始使用”。
 
   ![入门](./media/security-center-get-started/get-started-upgrade-tab.png)

    “升级”选项卡列出了符合加入条件的订阅和工作区。

1. 从“选择要对其启用标准层的工作区”列表中，选择要升级的工作区。


    > [!TIP]
    > 如果你选择了一个符合试用条件的工作区，下一步将开始试用。 如果工作区不符合试用条件，则对其进行升级并开始收费。

1. 选择“升级”，将所选工作区升级到标准层。


## <a name="automate-data-collection"></a>自动收集数据
安全中心从 Azure VM 和非 Azure 计算机收集数据以监视安全漏洞和威胁。 数据是使用 Log Analytics 代理收集的，该代理从计算机中读取各种与安全相关的配置和事件日志，然后将数据复制到工作区进行分析。 默认情况下，安全中心会自动创建新工作区。

启用自动预配后，安全中心可在所有受支持的 Azure VM 以及任何新建的 Azure VM 中安装 Log Analytics 代理。 我们强烈建议启用自动预配。

若要启用对 Log Analytics 代理的自动预配，请执行以下操作：

1. 在“安全中心”主菜单下，选择“定价和设置”  。
1. 在订阅的行上，单击要更改其设置的订阅。
1. 在“数据收集”  选项卡上，将“自动预配”  设置为“开启”。
1. 选择“保存”。 
---
  ![启用自动设置][6]

根据针对 Azure VM 生成的这些新见解，安全中心可以提供与系统更新状态、OS 安全配置、终结点保护相关的其他建议，并生成其他安全警报。

  ![建议][8]

## <a name="clean-up-resources"></a>清理资源
本系列中的其他快速入门和教程是在本快速入门的基础上制作的。 如果打算继续学习后续的快速入门和教程，请继续运行标准层并让自动预配保持启用状态。 如果不打算继续或想要返回到“免费”层，请执行以下操作：

1. 返回到“安全中心”主菜单，选择“定价和设置”。
2. 单击要更改为免费层的订阅。
3. 选择“定价层”并选择“免费”，将订阅从“标准”层更改为“免费”层。 
5. 选择“保存”  。

如果希望禁用自动预配，请执行以下操作：

1. 返回到“安全中心”主菜单，选择“定价和设置”。
2. 清理要禁用自动预配的订阅。
3. 在“数据收集”  选项卡上，将“自动预配”  设置为“关闭”。
4. 选择“保存”。 

>[!NOTE]
> 禁用自动预配不会从已预配代理的 Azure VM 中删除 Log Analytics 代理。 禁用自动设置会限制对资源的安全监视。
>

## <a name="next-steps"></a>后续步骤
在本快速入门中，你已升级到标准层，并预配了 Log Analytics 代理，以便对混合云工作负荷进行统一的安全管理和威胁防护。 若要详细了解如何使用安全中心，请继续学习有关如何加入本地和其他云中的 Windows 计算机的快速入门。

> [!div class="nextstepaction"]
> [快速入门：将 Windows 计算机加入安全中心](quick-onboard-windows-computer.md)

<!--Not available in MC: cost-management-billing/costs/quick-acm-cost-analysis -->
<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/get-started.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
[9]: ./media/security-center-get-started/select-subscription.png

