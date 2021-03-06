---
title: 使用 Site Recovery 将 Azure VMware 解决方案 VM 故障转移到 Azure
description: 了解如何在 Azure Site Recovery 中将 Azure VMware 解决方案 VM 故障转移到 Azure
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
origin.date: 09/30/2020
author: rockboyfor
ms.date: 11/09/2020
ms.testscope: yes
ms.testdate: 11/09/2020
ms.author: v-yeche
ms.custom: MVC
ms.openlocfilehash: 362d986fa0e7dca741f91ac8a2509fc73d00a02a
ms.sourcegitcommit: 6b499ff4361491965d02bd8bf8dde9c87c54a9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94329142"
---
<!--Verified Successfully-->
# <a name="fail-over--azure-vmware-solution-vms"></a>对 Azure VMware 解决方案 VM 进行故障转移

本文介绍如何使用 [Azure Site Recovery](site-recovery-overview.md) 将 Azure VMware 解决方案 VM 故障转移到 Azure。

本文是系列教程的第五篇文章，介绍如何为 Azure VMware 解决方案 VM 设置到 Azure 的灾难恢复。

在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]

> * 验证 Azure VMware 解决方案 VM 属性是否符合 Azure 要求。
> * 将特定 VM 故障转移到 Azure。

> [!NOTE]
> 教程介绍了某个方案的最简单部署路径。 它们尽可能地使用默认选项，并且不显示所有可能的设置和路径。 若要详细了解故障转移，请参阅[对 VM 进行故障转移](site-recovery-failover.md)。

[了解](failover-failback-overview.md#types-of-failover)不同类型的故障转移。 如果要在恢复计划中对多个 VM 进行故障转移，请查看[本文](site-recovery-failover.md)。

## <a name="before-you-start"></a>开始之前

完成前一篇教程：

1. 确保已为到 Azure 的灾难恢复[设置 Azure](avs-tutorial-prepare-azure.md)。
2. 遵循[这些步骤](avs-tutorial-prepare-avs.md)准备本地 Azure VMware 解决方案部署，以便能够灾难恢复到 Azure。
3. [设置](avs-tutorial-replication.md) Azure VMware 解决方案 VM 的灾难恢复。
4. 运行[灾难恢复演练](avs-tutorial-dr-drill-azure.md)，以确保一切按预期方式进行。

## <a name="verify-vm-properties"></a>验证 VM 属性

运行故障转移之前，请检查 VM 属性，确保 VM 符合 [Azure 要求](vmware-physical-azure-support-matrix.md#replicated-machines)。

按如下所述检查属性：

1. 在“受保护的项”中选择“复制的项”，然后选择要验证的 VM   。

2. “复制的项”窗格中具有 VM 信息、运行状况状态和最新可用恢复点的摘要  。 选择“属性”以查看更多详细信息  。

3. 可以在“计算和网络”中根据需要修改这些属性  ：
    * Azure 名称
    * 资源组
    * 目标大小
    * [可用性集](../virtual-machines/windows/tutorial-availability-sets.md)
    * 托管磁盘设置

4. 可以查看和修改网络设置，包括：

    * 故障转移后用于放置 Azure VM 的网络和子网。
    * 要分配给它的 IP 地址。

5. 在“磁盘”  中，可以看到关于 VM 上的操作系统和数据磁盘的信息。

## <a name="run-a-failover-to-azure"></a>运行到 Azure 的故障转移

1. 在“设置” > “复制的项”中选择要故障转移的 VM，然后选择“故障转移”  。
2. 在“故障转移”  中，选择要故障转移到的“恢复点”  。 可以使用以下选项之一：
   * **最新** ：此选项会首先处理发送到 Site Recovery 的所有数据。 它提供最低的恢复点目标 (RPO)，因为故障转移后创建的 Azure VM 具有触发故障转移时复制到 Site Recovery 的所有数据。
   * **最新处理** ：此选项将 VM 故障转移到由 Site Recovery 处理的最新恢复点。 此选项提供较低的 RTO（恢复时间目标），因为无需费时处理未经处理的数据。
   * **最新的应用一致** ：此选项将 VM 故障转移到由 Site Recovery 处理的最新应用一致恢复点。
   * **自定义** ：使用此选项可以指定恢复点。

3. 选择“在开始故障转移之前关闭计算机”，在触发故障转移之前尝试关闭源 VM  。 即使关机失败，故障转移也仍会继续。 可以在“作业”页上跟踪故障转移进度。 

在某些情况下，故障转移需要大约 8 到 10 分钟的时间完成其他进程。 对于以下情况，你可能会发现测试故障转移会持续较长时间：

* VMware VM 运行的移动服务版本低于 9.8。
* VMware Linux VM。
* VMware VM 未启用 DHCP 服务。
* VMware VM 不包含以下启动驱动程序：storvsc、vmbus、storflt、intelide、atapi。

> [!WARNING]
> 不会取消正在进行的故障转移。 在故障转移开始前，VM 复制已停止。 如果取消正在进行的故障转移，故障转移会停止，但 VM 将不再进行复制。

## <a name="connect-to-failed-over-vm"></a>连接到故障转移的 VM

1. 如果想在故障转移后通过使用远程桌面协议 (RDP) 和安全外壳 (SSH) 连接到 Azure VM，请[验证是否符合要求](failover-failback-overview.md#connect-to-azure-after-failover)。
2. 故障转移后，请转到该 VM，并通过与它建立[连接](../virtual-machines/windows/connect-logon.md)来进行验证。
3. 若要在故障转移后使用不同的恢复点，请使用“更改恢复点”。  在下一步骤中提交故障转移后，此选项不再可用。
4. 验证后，选择“提交”以确认故障转移后的 VM 恢复点  。
5. 提交后，系统会删除其他所有可用的恢复点。 完成此步骤，就完成了故障转移。

>[!TIP]
> 如果故障转移后遇到任何连接问题，请遵循[故障排除指南](site-recovery-failover-to-azure-troubleshoot.md)予以解决。

## <a name="next-steps"></a>后续步骤

故障转移后，重新保护 VM 并将其复制到 Azure VMware 解决方案私有云。 重新保护 VM 并将其复制到 Azure VMware 解决方案私有云之后，如果已准备就绪，请从 Azure 进行故障恢复。

> [!div class="nextstepaction"]
> [重新保护 Azure VM](avs-tutorial-reprotect.md)
> [从 Azure 进行故障恢复](avs-tutorial-failback.md)

<!-- Update_Description: new article about avs tutorial failover -->
<!--NEW.date: 11/09/2020-->