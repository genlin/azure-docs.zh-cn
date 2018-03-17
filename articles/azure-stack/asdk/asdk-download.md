---
title: "下载并提取 Azure 堆栈开发工具包 (ASDK) |Microsoft 文档"
description: "描述如何下载并提取 Azure 堆栈开发工具包 (ASDK)。"
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 0cf389c9443a9cff477b884c277d72de27769afc
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2018
---
# <a name="download-and-extract-the-azure-stack-development-kit-asdk"></a>下载并提取 Azure 堆栈开发工具包 (ASDK)
确保你开发工具包的主机计算机满足安装 ASDK 的基本要求之后下, 一步是要下载并解压缩 ASDK 部署包，以获取 Cloudbuilder.vhdx。

## <a name="download-the-asdk"></a>下载 ASDK
1. 开始下载之前，请确保计算机满足以下先决条件：

  - 计算机必须有至少 60 GB 的可用磁盘空间，这些空间位于除操作系统磁盘外的四个独立且相同的逻辑硬盘驱动器上。
  - 必须已安装 [.NET Framework 4.6（或更高版本）](https://aka.ms/r6mkiy)。

2. [转到“入门”页](https://azure.microsoft.com/overview/azure-stack/try/?v=try)，以便在其中下载 Azure Stack 开发工具包，提供自己的详细信息，然后单击“提交”。
3. 下载并运行先决条件检查器脚本：[用于 Azure Stack 开发工具包的部署检查器](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409)。 此独立脚本完成由 Azure Stack 开发工具包的安装程序执行的先决条件检查。 在下载更大的用于 Azure Stack 开发工具包的程序包之前，可以通过它来确认硬件和软件要求是否已得到满足。
4. 在“下载软件”下单击“Azure Stack 开发工具包”。

  > [!NOTE]
  > ASDK 下载 (AzureStackDevelopmentKit.exe) 为大约 10 GB。

## <a name="extract-the-asdk"></a>提取 ASDK
1. 下载完成后，请单击“运行”，启动 ASDK 自解压缩程序 (AzureStackDevelopmentKit.exe)。
2. 查看并接受自解压缩程序向导的“许可协议”页中显示的许可协议，然后单击“下一步”。
3. 查看自解压缩程序向导的“重要说明”页上显示的隐私声明信息，然后单击“下一步”。
4. 在自解压缩程序向导的“选择目标位置”页上选择要将 Azure Stack 安装程序文件提取到其中的位置，然后单击“下一步”。 默认位置为：当前文件夹\Azure Stack Development Kit。 
5. 查看自解压缩程序向导的“准备提取”页上的目标位置摘要，然后单击“提取”以提取 CloudBuilder.vhdx（约 25GB）和 ThirdPartyLicenses.rtf 文件。 此过程需要一些时间才能完成。
6. 将 CloudBuilder.vhdx 文件复制或移动到 ASDK 主机上的 C:\ 驱动器的根目录 (C:\CloudBuilder.vhdx)。

> [!NOTE]
> 提取这些文件以后，即可通过删除 .EXE 和 .BIN 文件来恢复硬盘空间。 或者，你可以备份这些文件，因此，不需要再次下载这些文件，如果你需要重新部署 ASDK。


## <a name="next-steps"></a>后续步骤
[准备 ASDK 主计算机](asdk-prepare-host.md)