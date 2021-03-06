---
title: 什么是 Azure Stack HCI 上的 Azure Kubernetes 服务？
description: Azure Stack HCI 上的 Azure Kubernetes 服务是大规模自动运行容器化应用程序的 Azure Kubernetes 服务 (AKS) 的本地实现。
ms.topic: overview
author: WenJason
ms.service: azure-stack
ms.author: v-jay
origin.date: 09/22/2020
ms.date: 10/12/2020
ms.openlocfilehash: 129e2e18048b6f0ef10f41a41b10e25a90e1a6c5
ms.sourcegitcommit: bc10b8dd34a2de4a38abc0db167664690987488d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2020
ms.locfileid: "91451221"
---
# <a name="what-is-azure-kubernetes-service-on-azure-stack-hci"></a>什么是 Azure Stack HCI 上的 Azure Kubernetes 服务？

Azure Stack HCI 上的 Azure Kubernetes 服务是大规模自动运行容器化应用程序的 Azure Kubernetes 服务 (AKS) 的本地实现。 Azure Kubernetes 服务现在在 Azure Stack HCI 上提供预览版，使你可以更快速地开始在数据中心内托管 Linux 和 Windows 容器。

若要开始在本地使用 Azure Kubernetes 服务，请[注册预览版](https://aka.ms/AKS-HCI-Evaluate)（预览版期间没有额外成本），然后参阅[设置 Azure Stack HCI 上的 Azure Kubernetes 服务](setup.md)。 若要改为使用 Azure Kubernetes 服务协调基于云的容器，请参阅 [Azure 中的 Azure Kubernetes 服务](/aks/intro-kubernetes)。

以下部分讨论了使用 Azure Stack HCI 上的 Azure Kubernetes 服务的一些原因，并回答了有关该服务以及如何开始使用的一些常见问题。 对应容器的背景，请参阅 [ 和容器](https://docs.microsoft.com/virtualization/windowscontainers/about/)。

## <a name="automate-management-of-containerized-applications"></a>自动管理容器化应用程序

尽管可以使用 Docker 和 Windows 手动管理一些容器，但应用通常使用五个、十个甚至数百个容器，因此 Kubernetes 业务流程协调程序应运而生。

Kubernetes 是一种开放源代码业务流程协调程序，用于大规模自动执行容器管理。 Azure Kubernetes 服务通过提供用于在 Azure Stack HCI 上设置 Kubernetes 和必要附加产品以及创建 Kubernetes 群集来托管工作负载的向导，简化了本地 Kubernetes 部署。

下面是在 Azure Stack HCI 上的预览版期间，Azure Kubernetes 服务提供的一些功能：

- 将容器化应用大规模部署到跨 Azure Stack HCI 群集运行的 VM 群集（称为 Kubernetes 群集）
- 当 Kubernetes 群集中的节点发生故障时进行故障转移
- 部署和管理基于 Linux 和 Windows 的容器化应用
- 计划工作负载
- 监视运行状况
- 通过在 Kubernetes 群集中添加或删除节点来纵向扩展或缩减
- 管理网络
- 发现服务
- 协调应用升级
- 通过群集节点相关性将 Pod 分配给群集节点

有关 Kubernetes 的详细信息，请参阅 [Kubernetes.io](https://kubernetes.io)。

## <a name="simplify-setting-up-kubernetes"></a>简化 Kubernetes 的设置

Azure Kubernetes 服务简化了在 Azure Stack HCI 上设置 Kubernetes 的过程，包括以下功能：

- 用于设置 Kubernetes 及其依赖项（例如 kubeadm、kubelet、kubectl 和 Pod 网络附加产品）的 Windows Admin Center 向导
- 用于创建 Kubernetes 群集以运行容器化应用程序的 Windows Admin Center 向导
- 用于设置 Kubernetes 和创建 Kubernetes 群集的 PowerShell cmdlet，以防为主机设置和 Kubernetes 群集创建编写脚本

## <a name="view-and-manage-kubernetes-using-on-premises-tools"></a>使用本地工具查看和管理 Kubernetes
设置 Azure Stack HCI 群集上的 Azure Kubernetes 服务并创建了 Kubernetes 群集后，我们提供了一种方法来管理和监视 Kubernetes 基础结构：

在本地使用常用工具（例如 Kubectl 和 Kubernetes 仪表板）- 使用开放源代码界面将应用程序部署到 Kubernetes 群集、管理群集资源、排除故障以及查看所运行的应用程序。
## <a name="run-linux-and-windows-containers"></a>运行 Linux 和 Windows 容器

Azure Kubernetes 服务完全支持基于 Linux 和基于 Windows 的容器。 在 Azure Stack HCI 上创建 Kubernetes 群集时，可以选择是否要创建节点池（相同 VM 的组）来运行 Linux 容器和/或 Windows 容器。 

Azure Kubernetes 服务会创建 Linux 和 Windows VM，以便无需直接管理 Linux 或 Windows 操作系统。

## <a name="secure-your-container-infrastructure"></a>保护容器基础结构

Azure Kubernetes 服务包含一些可帮助保护容器基础结构的功能：

- 工作器节点的基于虚拟机监控程序的隔离 - 每个 Kubernetes 群集都在其自己的专用和隔离虚拟机集上运行，以便租户可以共享相同的物理基础结构。
- Microsoft 维护的工资器节点的 Linux 和 Windows 映像 - 工作器节点运行 Microsoft 创建的 Linux 和 Windows 虚拟机映像，以遵循安全最佳做法。 Microsoft 还会使用最新的安全更新，每月刷新一次这些映像。

安全是 Azure Stack HCI 上的 Azure Kubernetes 服务预览版本的一个持续投资领域，因此请继续关注。

## <a name="where-can-i-run-azure-kubernetes-service"></a>在哪里可以运行 Azure Kubernetes 服务？

Azure Kubernetes 服务在以下平台上提供：

- 在 Azure 云中运行（通过 [Azure 中的 Azure Kubernetes 服务](/aks/intro-kubernetes)提供）
- 通过 Azure Stack HCI 上的 Azure Kubernetes 服务在本地运行（本文所介绍的内容）
- 使用 [Azure Stack Hub 上的 AKS 引擎](../user/azure-stack-kubernetes-aks-engine-overview.md)在 Azure Stack Hub 环境中本地运行。

## <a name="how-does-kubernetes-work-on-azure-stack-hci"></a>Kubernetes 在 Azure Stack HCI 上如何工作？

在 Azure Stack HCI 上运行时，Azure Kubernetes 服务的工作方式与在 Azure 云中使用它时稍有不同：

- Azure 中的 Kubernetes 服务是一种托管服务，其中会为你管理大部分 Kubernetes 管理基础结构（控制平面）。 控制平面和容器化应用程序都在 Azure 虚拟机中运行。
- 对于 Azure Stack HCI 上的 Azure Kubernetes 服务，你可以直接在 Azure Stack HCI 群集上设置该服务，可以说是使你可对控制平面进行控制。 控制平面、容器化应用程序和 Azure Kubernetes 服务本身全都在超融合群集托管的虚拟机中运行。

在 Azure Stack HCI 群集上设置了 Azure Kubernetes 服务后，其工作方式类似于托管 Azure Kubernetes 服务：你使用该服务创建运行容器化应用程序的 Kubernetes 群集。 这些 Kubernetes 群集是充当工作器节点（运行应用程序容器）的 VM 组。 Kubernetes 群集还包含控制平面，它由用于协调应用程序容器的 Kubernetes 系统服务组成。

下面是几个简化关系图，显示 Azure Kubernetes 服务的体系结构在 Azure 中运行时与在 Azure Stack HCI 中的比较。

:::image type="content" source="media\overview\aks-azure-architecture.png" alt-text="Azure 中托管的 Azure Kubernetes 服务的体系结构，其中显示平台服务和大部分控制平面如何由 Azure 进行管理，而运行容器化应用程序的 Kubernetes 群集由客户进行管理。" lightbox="media\overview\aks-azure-architecture.png":::

:::image type="content" source="media\overview\aks-hci-architecture.png" alt-text="Azure 中托管的 Azure Kubernetes 服务的体系结构，其中显示平台服务和大部分控制平面如何由 Azure 进行管理，而运行容器化应用程序的 Kubernetes 群集由客户进行管理。" lightbox="media\overview\aks-hci-architecture.png":::

## <a name="what-you-need-to-get-started"></a>入门所需操作

以下部分总结了运行 Azure Stack HCI 上的 Azure Kubernetes 服务所需的内容。 有关完整详细信息，请参阅[安装 Azure Stack HCI 上的 Azure Kubernetes 服务之前](system-requirements.md)。

### <a name="on-your-windows-admin-center-system"></a>在 Windows Admin Center 系统上

Windows Admin Center 管理系统具有以下要求：

- Windows 10（目前不支持 Windows Admin Center 服务器）
- 60 GB 可用空间
- 已向 Azure 注册
- 与 Azure Stack HCI 群集处于同一个域中

### <a name="on-the-azure-stack-hci-cluster-that-hosts-azure-kubernetes-service"></a>在托管 Azure Kubernetes 服务的 Azure Stack HCI 群集上

运行 Azure Stack HCI 版本 20H2 或更高版本的群集具有以下要求：

- 此预览版本的群集中最多有四台服务器
- 用于 Azure Kubernetes 服务的存储池中有 1 TB 可用容量
- 至少有 30 GB 可用内存用于运行 Azure Kubernetes 服务 VM
- 对于此预览版本，群集中的所有服务器都必须使用 EN-US 区域和语言选择

有关常规 Azure Stack HCI 要求，请参阅[部署 Azure Stack HCI之前](../hci/deploy/before-you-start.md)。

### <a name="the-network-configuration-for-azure-stack-hci"></a>Azure Stack HCI 的网络配置

连接到 Azure Stack HCI 群集上的 VM 的网络需要可供 Azure Kubernetes 服务使用并且可由 Azure Stack HCI 群集上的 VM 访问的专用 DHCP IPv4 地址范围

对于 Azure Stack HCI 上的 Azure Kubernetes 服务，不能在网络上使用 VLAN 标记。 对 Azure Stack HCI 和 Azure Kubernetes 服务 VM 使用的网络使用网络交换机上的访问（未标记）端口。

## <a name="next-steps"></a>后续步骤

若要开始使用 Azure Stack HCI 上的 Azure Kubernetes 服务，请参阅以下文章：

- [审查要求](system-requirements.md)
- [设置 Azure Stack HCI 上的 Azure Kubernetes 服务](create-kubernetes-cluster.md)
