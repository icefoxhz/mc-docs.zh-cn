---
title: 从自动化帐户启用 Azure 自动化更新管理
description: 本文介绍如何从自动化帐户启用更新管理。
services: automation
origin.date: 09/09/2020
ms.date: 09/28/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 9da6fd0ffeafc8760d8b1b876e02263a01bd15bc
ms.sourcegitcommit: b9dfda0e754bc5c591e10fc560fe457fba202778
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91246455"
---
# <a name="enable-update-management-from-an-automation-account"></a>从自动化帐户启用更新管理

本文介绍如何使用自动化帐户为环境中的 VM 启用[更新管理](update-mgmt-overview.md)功能。 要大规模启用 Azure VM，必须使用更新管理启用现有 Azure VM。

> [!NOTE]
> 在启用更新管理时，只有某些区域支持链接 Log Analytics 工作区和自动化帐户。 有关支持的映射对的列表，请参阅[自动化帐户和 Log Analytics 工作区的区域映射](../how-to/region-mappings.md)。

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果还没有帐户，可注册一个[试用帐户](https://wd.azure.cn/pricing/1rmb-trial-full)。
* 用于管理计算机的[自动化帐户](../index.yml)。
* [虚拟机](../../virtual-machines/windows/quick-create-portal.md)。

## <a name="sign-in-to-azure"></a>登录 Azure

登录 [Azure 门户](https://portal.azure.cn)。

## <a name="enable-update-management"></a>启用更新管理

1. 在自动化帐户中，选择“更新管理”下的“更新管理”。

2. 选择 Log Analytics 工作区和自动化帐户，然后选择“启用”以启用更新管理。 安装最多需要 15 分钟才能完成。

    ![启用更新管理](media/update-mgmt-enable-automation-account/onboardsolutions2.png)

## <a name="enable-azure-vms"></a>启用 Azure VM

1. 从自动化帐户中，选择“更新管理”下的“更新管理”。

2. 选择“+添加 Azure VM”，再从列表中选择一个或多个 VM。 无法启用的虚拟机将灰显，无法选择。 Azure VM 可以位于任何区域中，无论自动化帐户位于哪里。

3. 选择“启用”，将所选 VM 添加到计算机组为此功能保存的搜索结果中。

    ![启用 Azure VM](media/update-mgmt-enable-automation-account/enable-azure-vms.png)

## <a name="enable-non-azure-vms"></a>启用非 Azure VM

需要手动添加 Azure 中没有的计算机。

1. 从自动化帐户中，选择“更新管理”下的“更新管理”。

2. 选择“添加非 Azure 计算机”。 此操作将打开一个新的浏览器窗口，其中包含[有关安装和配置适用于 Windows 的 Log Analytics 代理的说明](../../azure-monitor/platform/log-analytics-agent.md)，使计算机可以开始向更新管理报告。 

## <a name="enable-machines-in-the-workspace"></a>在工作区中启用计算机

必须将手动安装的计算机或已向工作区报告的计算机添加到 Azure 自动化中，才能启用更新管理。

1. 从自动化帐户中，选择“更新管理”下的“更新管理”。

2. 选择“管理计算机”。 如果之前选择了“在所有可用的和将来的计算机上启用”选项，则“管理计算机”按钮可能灰显 

    ![保存的搜索](media/update-mgmt-enable-automation-account/managemachines.png)

3. 要为向工作区报告的所有可用计算机启用更新管理，请在“管理计算机”页上选择“在所有可用计算机上启用”。 此操作禁用单独添加计算机的控件。 此任务会将向工作区报告的所有计算机的名称添加到计算机组保存的搜索查询 `MicrosoftDefaultComputerGroup`。 选中此项后，此操作将禁用“管理计算机”按钮。

4. 若要为所有可用的计算机和将来的计算机启用该功能，请选择“在所有可用的和将来的计算机上启用”。 此选项从工作区中删除已保存的搜索和作用域配置，并允许该功能包括当前或将来向工作区报告的所有 Azure 和非 Azure 计算机。 选中此项后，此操作将永久禁用“管理计算机”按钮，因为没有可用的作用域配置。

    > [!NOTE]
    > 由于此选项会删除 Log Analytics 中保存的搜索和作用域配置，因此在选择此选项之前，必须先删除 Log Analytics 工作区中的所有删除锁。 否则，该选项将无法删除配置，必须手动将其删除。

5. 如果需要，可以通过重新添加初始的已保存搜索查询来添加作用域配置。 有关详细信息，请参阅[限制更新管理的部署范围](update-mgmt-scope-configuration.md)。

6. 若要为一台或多台计算机启用该功能，请选择“在所选计算机上启用”，并选择每台计算机旁边的“添加” 。 此任务会将所选计算机名称添加到计算机组为此功能保存的搜索查询。

## <a name="next-steps"></a>后续步骤

* 若要对 VM 使用更新管理，请参阅[管理 VM 的更新和修补程序](update-mgmt-manage-updates-for-vm.md)。

* 当不再需要通过更新管理来管理 VM 或服务器时，请参阅[从更新管理中删除 VM](update-mgmt-remove-vms.md)。

* 若要对常规更新管理错误进行故障排除，请参阅[排查更新管理问题](../troubleshoot/update-management.md)。