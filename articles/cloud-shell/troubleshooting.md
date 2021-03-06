---
title: Azure Cloud Shell 故障排除 | Microsoft Docs
description: Azure Cloud Shell 故障排除
services: azure
documentationcenter: ''
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: damaerte
ms.openlocfilehash: 3c01a31eae2b90ecb54cbfba7f565fd140db3773
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/19/2018
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Azure Cloud Shell 的故障排除和限制

排查 Azure Cloud Shell 中问题的已知解决方案包括：

## <a name="general-troubleshooting"></a>常规故障排除

### <a name="early-timeouts-in-firefox"></a>FireFox 中的提前超时
- **详细信息**：Cloud Shell 利用打开的 Websocket 将输入/输出传递到你的浏览器。 FireFox 已经预设了可提前关闭 websocket 的策略，导致在 Cloud Shell 中提前超时。
- **解决方法**：打开 FireFox 并在 URL 框中导航到“about:config”。 搜索“network.Websocket.timeout.ping.request”并将值从 0 更改为 10。

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>存储对话框 - 错误：403 RequestDisallowedByPolicy
- **详细信息**：通过 Cloud Shell 创建存储帐户时，由于管理员设置的 Azure 策略而失败。错误消息将包括：`The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **解决方法**：与 Azure 管理员联系，让其删除或更新拒绝存储创建的 Azure 策略。

### <a name="storage-dialog---error-400-disallowedoperation"></a>存储对话框 - 错误：400 DisallowedOperation
 - **详细信息**：使用 Azure Active Directory 订阅时，无法创建存储。
 - **解决方法**：使用能够创建存储资源的 Azure 订阅。 Azure AD 订阅无法创建 Azure 资源。

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>终端输出 - 错误：无法连接终端：无法建立 websocket。 按 `Enter` 重新连接。
 - **详细信息**：Cloud Shell 需要能够与 Cloud Shell 基础结构建立 websocket 连接。
 - **解决方法**：检查是否已将网络设置配置为允许向域（*.console.azure.com）发送 https 请求和 websocket 请求。

## <a name="bash-troubleshooting"></a>Bash 故障排除

### <a name="cannot-run-az-login"></a>无法运行 az login

- **详细信息**：无法运行 `az login`，原因是已在用于登录 Cloud Shell 或 Azure 门户的帐户下进行身份验证。
- **解决方法**：利用用于登录或注销的帐户并对目标 Azure 帐户重新进行身份验证。

### <a name="cannot-run-the-docker-daemon"></a>无法运行 docker 守护程序

- **详细信息**：Cloud Shell 利用容器托管 shell 环境，因此不允许运行守护程序。
- **解决方法**：利用默认安装的 [docker-machine](https://docs.docker.com/machine/overview/) 管理远程 Docker 主机中的 docker 容器。

## <a name="powershell-troubleshooting"></a>PowerShell 故障排除

### <a name="no-home-directory-persistence"></a>$Home 目录没有持久性

- **详细信息**：应用程序（例如 git、vim，等等）向 `$Home` 写入的任何数据未在 PowerShell 会话之间持久保留。
- **解决方法**：在 PowerShell 配置文件中，在 `clouddrive` 中创建指向 $Home 的应用程序特定文件夹的符号链接。

### <a name="ctrlc-doesnt-exit-out-of-a-cmdlet-prompt"></a>按 Ctrl+C 不会退出 Cmdlet 提示符

- **详细信息**：尝试退出 Cmdlet 提示符时，按 `Ctrl+C` 不会退出提示符。
- **解决方法**：若要退出提示符，请依次按 `Ctrl+C` 和 `Enter`。

### <a name="gui-applications-are-not-supported"></a>不支持 GUI 应用程序

- **详细信息**：如果用户启动 GUI 应用，提示符不会返回。 例如，当用户克隆已启用双因素身份验证的专用 GitHub 存储库时，会显示用于完成双因素身份验证的对话框。  
- **解决方法**：关闭再重新打开 shell。

### <a name="get-help--online-does-not-open-the-help-page"></a>运行 Get-help -online 不会打开帮助页

- **详细信息**：如果用户键入 `Get-Help Find-Module -online`，会看到如下所示的错误消息：`Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **解决方法**：复制 URL，并在浏览器上手动将其打开。

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Azure VM 的远程管理故障排除

- **详细信息**：由于 WinRM 的默认 Windows 防火墙设置，用户可能会看到以下错误：`Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **解决方法**：确保 VM 正在运行。 可以运行 `Get-AzureRmVM -Status` 确定 VM 状态。  接下来，在远程 VM 上添加新的防火墙规则，允许从任何子网建立 WinRM 连接，例如，

 ``` Powershell
 New-NetFirewallRule -Name 'WINRM-HTTP-In-TCP-PSCloudShell' -Group 'Windows Remote Management' -Enabled True -Protocol TCP -LocalPort 5985 -Direction Inbound -Action Allow -DisplayName 'Windows Remote Management - PSCloud (HTTP-In)' -Profile Public
 ```
 可以使用 [Azure custom script extension](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript) 来避免在添加新防火墙规则时登录到远程 VM。
 可将上述脚本保存在某个文件（例如 `addfirerule.ps1`）中，并将其上传到 Azure 存储容器。
 然后尝试以下命令：

 ``` Powershell
 Get-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup | Set-AzureRmVMCustomScriptExtension -VMName MyVM1 -FileUri https://mystorageaccount.blob.core.windows.net/mycontainer/addfirerule.ps1 -Run 'addfirerule.ps1' -Name myextension
 ```

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir` 在 Azure 驱动器中缓存结果

- **详细信息**：在 Azure 驱动器中缓存 `dir` 的结果。
- **解析**：在 Azure 驱动器视图中创建或删除资源后，运行 `dir -force` 以更新。

## <a name="general-limitations"></a>一般限制
Azure Cloud Shell 有以下已知限制：

### <a name="system-state-and-persistence"></a>系统状态和持久性

提供 Cloud Shell 会话的计算机是暂时性的，在会话处于非活动状态 20 分钟后会被回收。 Cloud Shell 需要装载 Azure 文件共享。 因此，订阅必须能够设置存储资源才能访问 Cloud Shell。 其他注意事项包括：

* 使用装载的存储时，仅持久保存 `clouddrive` 目录中的修改。 在 Bash 中，`$Home` 目录也会持久保存。
* 仅可从[已分配区域](persisting-shell-storage.md#mount-a-new-clouddrive)内部装载 Azure 文件共享。
  * 在 Bash 中，运行 `env` 可以找到设置为 `ACC_LOCATION` 的区域。
* Azure 文件仅支持本地冗余存储和异地冗余存储帐户。

### <a name="browser-support"></a>浏览器支持

Cloud Shell 支持最新版本的 Microsoft Edge、Microsoft Internet Explorer、Google Chrome、Mozilla Firefox 和 Apple Safari。 不支持专用模式下的 Safari。

### <a name="copy-and-paste"></a>复制和粘贴

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>对于一个给定的用户，只有一个 shell 可处于活动状态

不管是在 **Bash** 还是 **PowerShell** 中，用户每次只能启动一种类型的 shell。 但是，每次可以运行 Bash 或 PowerShell 的多个实例。 在 Bash 和 PowerShell 之间切换会导致 Cloud Shell 重启，从而终止现有会话。

### <a name="usage-limits"></a>使用限制

Cloud Shell 适用于交互式用例。 因此，任何长时间运行的非交互式会话都会在没有预警的情况下终止。

## <a name="bash-limitations"></a>Bash 限制

### <a name="user-permissions"></a>用户权限

权限设置为普通用户，不具有 sudo 访问权限。 不会保留 `$Home` 目录外部的任何安装。

### <a name="editing-bashrc"></a>编辑 .bashrc

编辑 .bashr 时要小心，执行这一操作可能导致 Cloud Shell 出现意外错误。

## <a name="powershell-limitations"></a>PowerShell 限制

### <a name="slow-startup-time"></a>启动速度缓慢

PowerShell in Azure Cloud Shell（预览版）最长可能需要 60 秒才能完成初始化。

### <a name="default-file-location-when-created-from-azure-drive"></a>从 Azure 驱动器创建时的默认文件位置：

使用 PowerShell cmdlet，用户无法在 Azure 驱动器下创建文件。 当用户使用其他工具（如 vim 或 nano）创建新文件时，文件将默认保存到 C:\Users 文件夹。 

### <a name="gui-applications-are-not-supported"></a>不支持 GUI 应用程序

如果用户运行一条会创建 Windows 对话框的命令（例如 `Connect-AzureAD` 或 `Connect-AzureRmAccount`），将看到如下所示的错误消息：`Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`。

## <a name="gdpr-compliance-for-cloud-shell"></a>Cloud Shell 的 GDPR 符合性

Azure Cloud Shell 非常重视你的个人数据，Azure Cloud Shell 服务捕获和存储的数据用于为你的体验提供默认值，例如最近使用的 shell、首选字号、首选字体类型和支持 clouddrive 的文件共享详细信息。 如果想要导出或删除此数据，我们添加了以下说明。

### <a name="export"></a>导出
若要导出 Cloud Shell 为你保存的用户设置（如首选 shell、字号和字体类型），请运行以下命令。

1. 启动 Cloud Shell 中的 Bash
2. 运行以下命令：
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
```

### <a name="delete"></a>删除
若要删除 Cloud Shell 为你保存的用户设置（如首选 shell、字号和字体类型），请运行以下命令。 下次启动 Azure Cloud Shell 时，系统会要求你再次载入文件共享。 

如果删除用户设置，不会删除实际的 Azure 文件共享。请转到“Azure 文件”完成该操作。

1. 启动 Cloud Shell 中的 Bash
2. 运行以下命令：
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
```
