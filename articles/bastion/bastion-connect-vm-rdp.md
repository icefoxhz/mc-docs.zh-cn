---
title: 使用 Azure Bastion 连接到 Windows VM
description: 本文介绍如何使用 Azure Bastion 连接到运行 Windows 的 Azure 虚拟机。
services: bastion
ms.service: bastion
ms.topic: how-to
origin.date: 10/12/2020
author: rockboyfor
ms.date: 11/02/2020
ms.testscope: yes
ms.testdate: 07/27/2020
ms.author: v-yeche
ms.openlocfilehash: f324eb2b77eef298a3f087bc9a7604098c20743b
ms.sourcegitcommit: 93309cd649b17b3312b3b52cd9ad1de6f3542beb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93106361"
---
<!--Verifed successfully on 09/07/2020-->
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>使用 Azure Bastion 连接到 Windows 虚拟机

使用 Azure Bastion，可以直接在 Azure 门户中通过 SSL 安全无缝地连接到你的虚拟机。 使用 Azure Bastion 时，VM 不需要客户端、代理或其他软件。 本文说明如何连接到 Windows VM。 若要了解如何连接到 Linux VM，请参阅[连接到 Linux VM](bastion-connect-vm-ssh.md)。

Azure Bastion 为预配它的虚拟网络中的所有 VM 提供安全的连接。 使用 Azure Bastion 可防止虚拟机向外部公开 RDP/SSH 端口，同时仍然使用 RDP/SSH 提供安全访问。 有关详细信息，请参阅[什么是 Azure Bastion？](bastion-overview.md)。

## <a name="prerequisites"></a>先决条件

在开始之前，请验证是否满足以下条件：

* 一个已安装 Bastion 主机的 VNet。

    请确保已为 VM 所在的虚拟网络设置 Azure Bastion 主机。 在虚拟网络中预配和部署 Bastion 服务后，便可以使用它连接到此虚拟网络中的任何 VM。 若要设置 Azure Bastion 主机，请参阅[创建堡垒主机](tutorial-create-host-portal.md#createhost)。
* 虚拟网络中的 Windows 虚拟机。
* 需要以下角色：
    * 虚拟机上的读者角色。
    * NIC 上的读者角色（使用虚拟机的专用 IP）。
    * Azure Bastion 资源上的读者角色。
* 端口：若要连接到 Windows VM，必须在 Windows VM 上打开以下端口：
    * 入站端口：RDP (3389)

<a name="rdp"></a>
## <a name="connect"></a>连接

[!INCLUDE [Connect to a Windows VM](../../includes/bastion-vm-rdp.md)]

## <a name="next-steps"></a>后续步骤

阅读 [Bastion 常见问题解答](bastion-faq.md)。

<!-- Update_Description: update meta properties, wording update, update link -->
