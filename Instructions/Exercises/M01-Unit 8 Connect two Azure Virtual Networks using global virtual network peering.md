---
Exercise:
  title: 模块 01 - 第 8 单元 使用全局虚拟网络对等互连连接两个 Azure 虚拟网络
  module: Module 01 - Introduction to Azure Virtual Networks
---

# 模块 01 - 第 8 单元 使用全局虚拟网络对等互连连接两个 Azure 虚拟网络

## 练习场景

在本单元中，你将通过添加对等互连来允许通信流，以配置 CoreServicesVnet 和 ManufacturingVnet 之间的连接。

![虚拟网络对等互连的示意图。](../media/8-exercise-connect-two-azure-virtual-networks-global.png)

### 工作技能

通过学习本练习，你将能够：

+ 任务 1：创建虚拟机来测试配置
+ 任务 2：使用 RDP 连接到测试 VM
+ 任务 3：测试 VM 间的连接
+ 任务 4：在 CoreServicesVnet 和 ManufacturingVnet 之间创建 VNet 对等互连
+ 任务 5：测试 VM 间的连接

### 交互式实验室模拟

>**注意**：此前提供的实验室模拟已停用。

### 预计用时：20 分钟

## 任务 1：创建虚拟机以测试配置

在本部分中，你将在 VNet 上创建一个测试 VM，用来测试是否可以从 Vnet 访问另一个 Azure 虚拟网络中的资源。

### 创建 ManufacturingVM

1. 在 Azure 门户中，选择右上角的 Cloud Shell 图标。 如有必要，请配置 Shell。  
    + 选择“PowerShell”****。
    + 选择“**不需要存储帐户**”和“**订阅**”，然后选择“**应用**”。
    + 等待终端创建并显示提示。 

1. 在 Cloud Shell 窗格的工具栏中，选择“**管理文件**”图标，在下拉菜单中选择“**上传**”，上传下列文件：**ManufacturingVMazuredeploy.json** 和 **ManufacturingVMazuredeploy.parameters.json**。

    >**备注：** 如果在自己的订阅中工作，则[模板文件](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/tree/master/Allfiles/Exercises)在 GitHub 实验室存储库中可用。

1. 部署以下 ARM 模板以创建此练习所需的 VM：

   >注意：系统会提示你提供管理员密码。

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
  
1. 部署完成后，转到 Azure 门户主页，然后选择“虚拟机”。

1. 验证是否已创建虚拟机。

## 任务 2：使用 RDP 连接到测试 VM

1. 在 Azure 门户主页上，选择“虚拟机”。

1. 选择“ManufacturingVM”。

1. 在“ManufacturingVM”中，选择“连接”&gt;“RDP”。

1. 在“ManufacturingVM \| 连接”中，选择“**下载 RDP 文件**”。

1. 将 RDP 文件保存到桌面。

1. 使用 RDP 文件和用户名 TestUser 以及部署期间提供的密码连接到 ManufacturingVM。

1. 在 Azure 门户主页上，选择“虚拟机”。

1. 选择“TestVM1”。

1. 在 TestVM1 中，选择“连接”&gt;“RDP”。

1. 在“TestVM1 \| 连接”中，选择“**下载 RDP 文件**”。

1. 将 RDP 文件保存到桌面。

1. 使用 RDP 文件和用户名 TestUser 以及部署期间提供的密码连接到 TestVM1。

1. 在这两个 VM 上的“选择设备的隐私设置”中，选择“接受”。

1. 在这两个 VM 上的“网络”中，选择“是”。

1. 在 TestVM1 上，打开 PowerShell 提示符，然后运行以下命令：ipconfig

1. 记录 IPv4 地址。

## 任务 3：测试 VM 间的连接

1. 在“ManufacturingVM”上，打开 PowerShell 提示符。

1. 使用以下命令验证 CoreServicesVnet 上是否不存在与 TestVM1 的连接。 请确保对 TestVM1 使用 IPv4 地址。

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```

1. 测试连接应失败，且你将看到如下所示的结果：![Test-NetConnection 10.20.20.4 - 端口 3389 显示失败的 PowerShell 窗口](../media/test-netconnection-fail.png)

## 任务 4：在 CoreServicesVnet 和 ManufacturingVnet 之间创建 VNet 对等互连

1. 在 Azure 主页上，选择“虚拟网络”，然后选择“CoreServicesVnet”。

1. 在“CoreServicesVnet”中的“设置”下，选择“对等互连”。
   ![核心服务 Vnet 对等互连设置的屏幕截图](../media/create-peering-on-coreservicesvnet.png)

1. 在“CoreServicesVnet”\|“对等互连”上，选择“+ 添加”****。

1. 使用此信息创建对等互连。 完成后，选择“添加”****。 

   远程虚拟网络摘要****

   | **选项**                                    | 值                             |
   | ------------------------------------ | --------------------------------------------- | 
   | 对等互连链接名称    | `ManufacturingVnet-to-CoreServicesVnet` |
   | 虚拟网络 | ManufacturingVnet |

    远程虚拟网络对等互连设置****
   
   | **选项**                                    | 值                             |
   | ------------------------------------ | --------------------------------------------- | 
   | 允许 'ManufacturingVnet' 访问 'CoreServicesVnet' | 已启用 |
   |允许 'ManufacturingVnet' 接收来自 'CoreServicesVnet' 的转发流量 | 已启用 |
 
    本地虚拟网络摘要****

    | **选项**                                    | 值                             |
    | ------------------------------------ | --------------------------------------------- | 
    | 对等互连链接名称 | `CoreServicesVnet-to-ManufacturingVnet` |
 
    远程虚拟网络对等互连设置****
   
    | **选项**                                    | 值                             |
    | ------------------------------------ | --------------------------------------------- | 
    | 允许 'CoreServicesVnet' 访问 'ManufacturingVnet' | 已启用
    | 允许 'CoreServicesVnet' 接收来自 'ManufacturingVnet' 的转发流量 | 已启用 |
 
1. 在 CoreServicesVnet \| 对等互连中，验证 **CoreServicesVnet-to-ManufacturingVnet** 对等互连是否**已连接**。

1. 在“虚拟网络”下，选择 **ManufacturingVnet**，然后验证 **CoreServicesVnet-to-ManufacturingVnet **对等互连是否**已连接**。

## 任务 5：测试 VM 间的连接

1. 在“ManufacturingVM”上，打开 PowerShell 提示符。

1. 使用以下命令验证 CoreServicesVnet 上现是否存在与 TestVM1 的连接。

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```

1. 测试连接应成功，且你将看到如下所示的结果：![Test-NetConnection 10.20.20.4 - 端口 3389 显示 TCP 测试成功: true 的 Powershell 窗口](../media/test-connection-succeeded.png)


## 清理资源

   >**注意**：记得删除所有不再使用的新建 Azure 资源。 删除未使用的资源可确保不会出现意外费用。

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话 。 （如有必要，使用默认设置创建 Cloud Shell 存储。）

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```powershell
   Remove-AzResourceGroup -Name 'ContosoResourceGroup' -Force -AsJob
   ```
   >**注意**：该命令以异步方式执行（由 -AsJob 参数决定），因此，虽然你可以随后立即在同一个 PowerShell 会话中运行另一个 PowerShell 命令，但需要几分钟才能实际删除资源组。
   
## 使用 Copilot 扩展学习

Copilot 可帮助你了解如何使用 Azure 脚本工具。 Copilot 还可以帮助了解实验室中未涵盖的领域或需要更多信息的领域。 打开 Edge 浏览器并选择“Copilot”（右上角）或导航到*copilot.microsoft.com*。 花几分钟时间尝试这些提示。
+ 配置 Azure 虚拟网络对等互连时最常见的错误是什么？
+ 在 Azure 中，如果我将 Vnet1 与 Vnet2 对等互连，然后将 Vnet2 与 Vnet3 对等互连，则 Vnet1 是否与 Vnet3 对等互连？
+ 防火墙和网关是否会影响 Azure 虚拟网络对等互连？


## 通过自定进度的培训了解详细信息

+ [Azure 虚拟网络的简介](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/)。 在本模块中，你将了解如何设计和实现 Azure 网络服务。 你将了解虚拟网络、公共和专用 IP、DNS、虚拟网络对等互连、路由和 Azure 虚拟 NAT。
+ [跨 Azure 虚拟网络分发服务并通过使用虚拟网络对等互连进行集成](https://learn.microsoft.com/training/modules/integrate-vnets-with-vnet-peering/)。 在本模块中，你将了解如何配置虚拟网络对等互连。

## 关键结论

恭喜你完成本实验室的内容。 下面是本实验室的主要重点。 

+ 借助虚拟网络对等互连，你可以无缝连接两个 Azure 虚拟网络。 出于连接目的，两个虚拟网络会显示为一个。
+ Azure 支持在同一 Azure 区域内或跨 Azure 区域（全局）连接虚拟网络。
+ 对等互连虚拟网络中虚拟机之间的流量直接通过 Microsoft 主干基础结构路由，而不通过网关或公共 Internet 路由。
+ 可以调整对等互连的 Azure 虚拟网络的地址空间大小，而不会导致当前对等互连的地址空间出现任何故障。
