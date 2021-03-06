---
title: 使用 Azure CLI 管理虚拟机规模集
description: 管理虚拟机规模集常用的 Azure CLI 命令，例如管理如何启动和停止实例，或更改规模集容量。
author: ju-shim
ms.author: v-junlch
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: management
ms.date: 10/20/2020
ms.reviewer: mimckitt
ms.custom: mimckitt, devx-track-azurecli
ms.openlocfilehash: 38dfa3161e7531da5e5df3d46562a45eba9ef456
ms.sourcegitcommit: 537d52cb783892b14eb9b33cf29874ffedebbfe3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92472021"
---
# <a name="manage-a-virtual-machine-scale-set-with-the-azure-cli"></a>使用 Azure CLI 管理虚拟机规模集
在虚拟机规模集的整个生命周期内，可能需要运行一个或多个管理任务。 此外，可能还需要创建自动执行各种生命周期任务的脚本。 本文详细介绍了执行这些任务常用的一些 Azure CLI 命令。

若要完成这些管理任务，需要最新的 Azure CLI。 请参阅[安装 Azure CLI](/cli/install-azure-cli) 了解相关信息。 如果需要创建虚拟机规模集，可以[使用 Azure CLI 创建规模集](quick-create-cli.md)。


## <a name="view-information-about-a-scale-set"></a>查看有关规模集的信息
若要查看有关规模集的全部信息，请使用 [az vmss show](/cli/vmss)。 以下示例将获取有关 myResourceGroup 资源组中 myScaleSet 规模集的信息 。 按如下所示输入自己的名称：

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
```


## <a name="view-vms-in-a-scale-set"></a>查看规模集中的 VM
要在规模集中查看 VM 实例的列表，请使用 [az vmss list-instances](/cli/vmss)。 以下示例将列出 myResourceGroup 资源组中 myScaleSet 规模集的所有 VM 实例。 为这些名称提供自己的值：

```azurecli
az vmss list-instances \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --output table
```

要查看与特定 VM 实例有关的其他信息，请将 `--instance-id` 参数添加到 [az vmss get-instance-view](/cli/vmss)，并指定要查看的实例。 以下示例将查看与 myScaleSet 规模集和 myResourceGroup 资源组中 VM 实例“0”有关的信息  。 按如下所示输入自己的名称：

```azurecli
az vmss get-instance-view \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --instance-id 0
```

还可以通过一个 API 调用获取所有实例的详细 instanceView 信息，这有助于避免大型安装的 API 限制。 为 `--resource-group`、`--subscription` 和 `--name` 提供你自己的值。

```azurecli
az vmss list-instances \
    --expand instanceView \
    --select instanceView \
    --resource-group <resourceGroupName> \
    --subscription <subID> \
    --name <vmssName>
```

```rest
GET "https://management.chinacloudapi.cn/subscriptions/<sub-id>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSSName>/virtualMachines?api-version=2019-03-01&%24expand=instanceView"
```

## <a name="list-connection-information-for-vms"></a>VM 连接信息列表
若要连接规模集中的 VM，可将 SSH 或 RDP 连接到分配的公共 IP 地址和端口号。 默认情况下，会向将远程连接流量转发给每个 VM 的 Azure 负载均衡器添加网络地址转换 (NAT) 规则。 若要列出规模集中连接 VM 的地址和端口，请使用 [az vmss list-instance-connection-info](/cli/vmss)。 以下示例将列出 myScaleSet 规模集和 myResourceGroup 资源组中 VM 实例的连接信息 。 为这些名称提供自己的值：

```azurecli
az vmss list-instance-connection-info \
    --resource-group myResourceGroup \
    --name myScaleSet
```


## <a name="change-the-capacity-of-a-scale-set"></a>更改规模集的容量
以上命令显示了与规模集和 VM 实例相关的信息。 要增加或减少规模集中的实例数，可以更改其容量。 规模集会创建或删除所需数量的 VM，然后配置 VM 以接收应用程序流量。

若要查看规模集中当前包含的实例数，请使用 [az vmss show](/cli/vmss) 并查询 sku.capacity：

```azurecli
az vmss show \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

然后，可以使用 [az vmss scale](/cli/vmss) 手动增加或减少规模集中虚拟机的数目。 以下示例将规模集中 VM 的数目设置为“5”：

```azurecli
az vmss scale \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --new-capacity 5
```

更新规模集容量需要花费数分钟。 如果减少规模集的容量，将首先删除实例 ID 最大的 VM。


## <a name="stop-and-start-vms-in-a-scale-set"></a>停止和启动规模集中的 VM
要在规模集中停止一个或多个 VM，请使用 [az vmss stop](/cli/vmss#az-vmss-stop)。 通过 `--instance-ids` 参数，可指定要停止的一个或多个 VM。 若不指定实例 ID，则停止规模集中的所有 VM。 要停止多个 VM，请用空格分隔每个实例 ID。

以下示例将停止 myScaleSet 规模集和 myResourceGroup 资源组中的实例“0”  。 请按照如下所示，提供值：

```azurecli
az vmss stop --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```

已停止的 VM 会保留已分配状态，且会继续产生计算费用。 如果想 VM 保持已解除分配状态，且仅会产生存储费用，使用 [az vmss deallocate](/cli/vmss)。 要解除分配多个 VM，请用空格分隔每个实例 ID。 以下示例停止并并解除分配 myScaleSet 规模集和 myResourceGroup 资源组中实例“0”  。 请按照如下所示，提供值：

```azurecli
az vmss deallocate --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


### <a name="start-vms-in-a-scale-set"></a>启动规模集中的 VM
要在规模集中启动一个或多个 VM，请使用 [az vmss start](/cli/vmss)。 通过 `--instance-ids` 参数，可指定要启动的一个或多个 VM。 若不指定实例 ID，则启动规模集中的所有 VM。 要启动多个 VM，请用空格分隔每个实例 ID。

以下示例将启动 myScaleSet 规模集和 myResourceGroup 资源组中的实例“0”  。 请按照如下所示，提供值：

```azurecli
az vmss start --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="restart-vms-in-a-scale-set"></a>重启规模集中的 VM
若要重启规模集中的一个或多个 VM，请使用 [az vmss restart](/cli/vmss)。 通过 `--instance-ids` 参数，可指定要重启的一个或多个 VM。 若不指定实例 ID，则重启规模集中的所有 VM。 要重启多个 VM，请用空格分隔每个实例 ID。

以下示例将重启 myScaleSet 规模集和 myResourceGroup 资源组中的实例“0”  。 请按照如下所示，提供值：

```azurecli
az vmss restart --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="remove-vms-from-a-scale-set"></a>删除规模集中的 VM
要在规模集中删除一个或多个 VM，请使用 [az vmss delete-instances](/cli/vmss)。 通过 `--instance-ids` 参数，可指定要删除的一个或多个 VM。 若指定实例 ID 的 *，规模集中所有 VM 都将删除。 要删除多个 VM，请用空格分隔每个实例 ID。

以下示例将删除 myScaleSet 规模集和 myResourceGroup 资源组中的实例“0”  。 请按照如下所示，提供值：

```azurecli
az vmss delete-instances --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="next-steps"></a>后续步骤
规模集的其他常见任务包括如何[部署应用程序](virtual-machine-scale-sets-deploy-app.md)和[升级 VM 实例](virtual-machine-scale-sets-upgrade-scale-set.md)。 也可使用 Azure CLI 来[配置自动缩放规则](virtual-machine-scale-sets-autoscale-overview.md)。

