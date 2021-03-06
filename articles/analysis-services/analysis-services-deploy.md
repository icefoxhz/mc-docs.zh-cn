---
title: 使用 Visual Studio 将模型部署到 Azure Analysis Services | Azure
description: 了解如何使用 Visual Studio 将表格模型部署到 Azure Analysis Services 服务器。
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 05/19/2020
author: rockboyfor
ms.date: 10/26/2020
ms.testscope: no
ms.testdate: 11/25/2019
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: e823a2f4213f698d142390b94ce0e0568766b567
ms.sourcegitcommit: 7b3c894d9c164d2311b99255f931ebc1803ca5a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92470146"
---
# <a name="deploy-a-model-from-visual-studio"></a>从 Visual Studio 部署模型

在 Azure 订阅中创建服务器之后，便可以开始将表格模型数据库部署到其中。 可以将 Visual Studio 与 Analysis Services 项目一起使用，以生成和部署要处理的表格模型项目。 

## <a name="prerequisites"></a>先决条件

要开始，需要：

* Azure 中的 **Analysis Services 服务器** 。 若要了解详细信息，请参阅[创建 Azure Analysis Services 服务器](analysis-services-create-server.md)。
* Visual Studio 中的 **表格模型项目** 或兼容级别为 1200 或更高的现有表格模型。 从未创建过？ 尝试 [Adventure Works Internet 销售表格建模教程](https://docs.microsoft.com/analysis-services/tutorial-tabular-1400/as-adventure-works-tutorial)。
* **本地网关** - 如果组织的网络中有一个或多个本地数据源，则需要安装 [本地数据网关](analysis-services-gateway.md)。 网关对于云中的服务器是必需的，可帮助服务器连接到本地数据源，以在模型中处理和刷新数据。

> [!TIP]
> 在部署之前，请确保可以在表中处理数据。 在 Visual Studio 中，单击“模型” > “处理” > “全部处理”。 如果处理失败，将无法成功部署。
> 
> 

## <a name="get-the-server-name"></a>获取服务器名称

在 **Azure 门户** 中，单击“服务器”>“概述” > “服务器名称”，并复制服务器名称。

:::image type="content" source="./media/analysis-services-deploy/aas-deploy-get-server-name.png" alt-text="在 Azure 中获取服务器名称":::

## <a name="to-deploy-from-visual-studio"></a>从 Visual Studio 部署

1. 在 Visual Studio >“解决方案资源管理器”中，右键单击项目，并选择“属性”。 在“部署” > “服务器”中，粘贴服务器名称。    

    :::image type="content" source="./media/analysis-services-deploy/aas-deploy-deployment-server-property.png" alt-text="在 Azure 中获取服务器名称":::

2. 在“解决方案资源管理器”中，右键单击“属性”，并单击“部署”。 系统可能会提示登录到 Azure。

    :::image type="content" source="./media/analysis-services-deploy/aas-deploy-deploy.png" alt-text="在 Azure 中获取服务器名称":::

    部署状态会在“输出”窗口和“部署”中显示。

    :::image type="content" source="./media/analysis-services-deploy/aas-deploy-status.png" alt-text="在 Azure 中获取服务器名称":::

就是这么简单！

## <a name="troubleshooting"></a>故障排除

如果部署元数据时部署失败，可能是因为 Visual Studio 无法连接到服务器。 确保可以使用 SQL Server Management Studio (SSMS) 连接到服务器。 确保项目的“部署服务器”属性正确。

如果部署在表上失败，可能是因为服务器无法连接到数据源。 如果数据源位于组织网络中的本地位置，请务必安装[本地数据网关](analysis-services-gateway.md)。

## <a name="next-steps"></a>后续步骤

现在，已将表格模型部署到服务器，可以与其进行连接了。 可以[使用 SQL Server Management Studio (SSMS) 连接到该服务器](analysis-services-manage.md)以对其进行管理。 并且，可以[使用客户端工具连接到它](analysis-services-connect.md)，如 Power BI、Power BI Desktop 或 Excel，并开始创建报表。

若要了解高级部署方法，请参阅[表格模型解决方案部署](https://docs.microsoft.com/analysis-services/deployment/tabular-model-solution-deployment?view=azure-analysis-services-current)。

<!-- Update_Description: update meta properties, wording update, update link -->