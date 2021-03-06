---
title: 配置 Always-On VPN 隧道
titleSuffix: Azure Virtual WAN
description: 为虚拟 WAN 配置 Always On VPN 设备隧道的步骤
services: virtual-wan
ms.service: virtual-wan
ms.topic: how-to
origin.date: 09/22/2020
author: rockboyfor
ms.date: 11/02/2020
ms.testscope: yes
ms.testdate: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: abbf604021e0ffb386b5a44059b2bf9a3d6840a2
ms.sourcegitcommit: 93309cd649b17b3312b3b52cd9ad1de6f3542beb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93103751"
---
<!--Verified successfully on VPN -->
# <a name="configure-an-always-on-vpn-device-tunnel-for-virtual-wan"></a>为虚拟 WAN 配置 Always On VPN 设备隧道

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>先决条件

必须创建点到站点配置并编辑虚拟中心分配。 有关说明，请参阅以下部分：

* [创建 P2S 配置](virtual-wan-point-to-site-portal.md#p2sconfig)
* [使用 P2S 网关创建中心](virtual-wan-point-to-site-portal.md#hub)

## <a name="configure-the-device-tunnel"></a>配置设备隧道

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>删除配置文件

若要删除配置文件，请运行以下命令：

:::image type="content" source="./media/howto-always-on-device-tunnel/cleanup.png" alt-text="屏幕截图显示了运行 Remove-VpnConnection -Name MachineCertTest 命令的 PowerShell 窗口。":::

## <a name="next-steps"></a>后续步骤

有关虚拟 WAN 的详细信息，请参阅[常见问题解答](virtual-wan-faq.md)。

<!-- Update_Description: update meta properties, wording update, update link -->