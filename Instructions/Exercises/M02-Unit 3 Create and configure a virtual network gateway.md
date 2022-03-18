---
Exercise:
  title: 模块 02 第 3 单元 - 创建和配置虚拟网络网关
  module: Module - Design and implement hybrid networking
ms.openlocfilehash: 85db6e283f4b1bfb8f57c110d3d3eadd9af2d755
ms.sourcegitcommit: f63ebaa31399a7b2b37abc32ed64f24f3d40608c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2022
ms.locfileid: "138028507"
---
# <a name="m02-unit-3-create-and-configure-a-virtual-network-gateway"></a>模块 02 第 3 单元 - 创建和配置虚拟网络网关

在本练习中，配置一个虚拟网关以连接 Contoso Core Services VNet 和 Manufacturing VNet。 

通过学习本练习，你将能够：

+ 任务 1：创建 CoreServicesVnet 和 ManufacturingVnet
+ 任务 2：创建 CoreServicesVM
+ 任务 3：创建 ManufacturingVM
+ 任务 4：使用 RDP 连接到测试 VM
+ 任务 5：测试 VM 间的连接
+ 任务 6：创建 CoreServicesVnet 网关
+ 任务 7：创建 ManufacturingVnet 网关
+ 任务 8：将 CoreServicesVnet 连接到 ManufacturingVnet 
+ 任务 9：将 ManufacturingVnet 连接到 CoreServicesVnet
+ 任务 10：验证连接是否成功 
+ 任务 11：测试 VM 间的连接

## <a name="task-1-create-coreservicesvnet-and-manufacturingvnet"></a>任务 1：创建 CoreServicesVnet 和 ManufacturingVnet

1. 在 Azure 门户的“Cloud Shell”窗格内打开“PowerShell”会话。

2. 在“Cloud Shell”窗格的工具栏中，单击“上传/下载文件”图标，在下拉菜单中单击“上传”，将 azuredeploy.json 和 azuredeploy.parameters.json 文件上传到 Cloud Shell 主目录中 。

3. 部署以下 ARM 模板来创建本练习所需的虚拟网络和子网：

   ```powershell
   $RGName = "ContosoResourceGroup"
   #create resource group if it doesnt exist
   New-AzResourceGroup -Name $RGName -Location "eastus"
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
   ```

## <a name="task-2-create-coreservicesvm"></a>任务 2：创建 CoreServicesVM

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

2. 在 Cloud Shell 窗格的工具栏中，单击“上传/下载文件”图标，在下拉菜单中单击“上传”，将文件 CoreServicesVMazuredeploy.json 和 CoreServicesVMazuredeploy.parameters.json 从源文件夹 F:\Allfiles\Exercises\M02 上传到 Cloud Shell 主目录  。

3. 部署以下 ARM 模板以创建此练习所需的 VM：

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile CoreServicesVMazuredeploy.json -TemplateParameterFile CoreServicesVMazuredeploy.parameters.json
   ```
  
4. 部署完成后，转到 Azure 门户主页，然后选择“虚拟机”。

5. 验证是否已创建虚拟机。

## <a name="task-3-create-manufacturingvm"></a>任务 3：创建 ManufacturingVM

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

2. 在 Cloud Shell 窗格的工具栏中，单击“上传/下载文件”图标，在下拉菜单中单击“上传”，将文件 ManufacturingVMazuredeploy.json 和 ManufacturingVMazuredeploy.parameters.json 从源文件夹 F:\Allfiles\Exercises\M02 上传到 Cloud Shell 主目录  。

3. 部署以下 ARM 模板以创建此练习所需的 VM：

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
  
4. 部署完成后，转到 Azure 门户主页，然后选择“虚拟机”。

5. 验证是否已创建虚拟机。


## <a name="task-4-connect-to-the-test-vms-using-rdp"></a>任务 4：使用 RDP 连接到测试 VM

1. 在 Azure 门户主页上，选择“虚拟机”。
2. 选择“ManufacturingVM”。
3. 在“ManufacturingVM”中，选择“连接”&gt;“RDP” 。
4. 在“ManufacturingVM | 连接”中，选择“下载 RDP 文件” 。
5. 将 RDP 文件保存到桌面。
6. 使用 RDP 文件连接到 ManufacturingTestVM，用户名为 TestUser，密码为 TestPa$$w0rd! 。 连接后，最小化 RDP 会话。
7. 在 Azure 门户主页上，选择“虚拟机”。
8. 选择 CoreServicesVM。
9. 在“CoreServicesTestVM”中，选择“连接”&gt;“RDP” 。
10. 在“CoreServicesTestVM | 连接”中，选择“下载 RDP 文件” 。
11. 将 RDP 文件保存到桌面。
12. 使用 RDP 文件连接到 CoreServicesTestVM，用户名为 TestUser，密码为 TestPa$$w0rd! 。
13. 在这两个 VM 上的“选择设备的隐私设置”中，选择“接受”。
14. 在这两个 VM 上的“网络”中，选择“是”。
15. 在 CoreServicesTestVM 上，打开 PowerShell 并运行此命令：ipconfig
16. 记录 IPv4 地址。 

 

## <a name="task-5-test-the-connection-between-the-vms"></a>任务 5：测试 VM 间的连接

1. 在“ManufacturingTestVM”上，打开 PowerShell。

2. 使用以下命令验证 CoreServicesVnet 上是否不存在与 CoreServicesTestVM 的连接。 请务必使用 CoreServicesTestVM 的 IPv4 地址。

   ```Powershell
   Test-NetConnection 10.20.20.4 -port 3389
   ```

3. 测试连接应失败，且你将看到如下所示的结果：

   ![Test-NetConnection 失败。](../media/test-netconnection-fail.png)

 

##  <a name="task-6-create-coreservicesvnet-gateway"></a>任务 6：创建 CoreServicesVnet 网关

1. 在“搜索资源、服务和文档(G+/)”中，输入“虚拟网关”，然后从结果中选择“虚拟网关”。
   ![在 Azure 门户上搜索虚拟网关。](../media/virtual-network-gateway-search.png)

2. 在“虚拟网关”中，选择“+ 创建”。

3. 使用下表中的信息创建虚拟网关：

   | Tab         | **节**       | **选项**                                  | **值**                    |
   | --------------- | ----------------- | ------------------------------------------- | ---------------------------- |
   | 基本信息          | 项目详细信息   | 订阅                                | 无需更改          |
   |                 |                   | ResourceGroup                               | ContosoResourceGroup         |
   |                 | 实例详细信息  | 名称                                        | CoreServicesVnetGateway      |
   |                 |                   | 区域                                      | 美国东部                      |
   |                 |                   | 网关类型                                | VPN                          |
   |                 |                   | VPN 类型                                    | 基于路由                  |
   |                 |                   | SKU                                         | VpnGw1                       |
   |                 |                   | Generation                                  | 第 1 代                  |
   |                 |                   | 虚拟网络                             | CoreServicesVnet             |
   |                 |                   | 子网                                      | GatewaySubnet (10.20.0.0/27) |
   |                 | 公共 IP 地址 | 公共 IP 地址                           | 新建                   |
   |                 |                   | 公共 IP 地址名称                      | CoreServicesVnetGateway-ip   |
   |                 |                   | 公用 IP 地址 SKU                       | 基本                        |
   |                 |                   | 启用主动-主动模式                   | 已禁用                     |
   |                 |                   | 配置 BGP                               | 已禁用                     |
   | 查看 + 创建 |                   | 检查设置，然后选择“创建”。 |                              |

   > [!NOTE] 
   >
   > 最多需要 45 分钟就可创建虚拟网络网关。 

## <a name="task-7-create-manufacturingvnet-gateway"></a>任务 7：创建 ManufacturingVnet 网关

1. 在“搜索资源、服务和文档(G+/)”中，输入“虚拟网关”，然后从结果中选择“虚拟网关”。

2. 在“虚拟网关”中，选择“+ 创建”。

3. 使用下表中的信息创建虚拟网关：

   | Tab         | **节**       | **选项**                                  | **值**                    |
   | --------------- | ----------------- | ------------------------------------------- | ---------------------------- |
   | 基本信息          | 项目详细信息   | 订阅                                | 无需更改          |
   |                 |                   | ResourceGroup                               | ContosoResourceGroup         |
   |                 | 实例详细信息  | 名称                                        | ManufacturingVnetGateway     |
   |                 |                   | 区域                                      | 西欧                  |
   |                 |                   | 网关类型                                | VPN                          |
   |                 |                   | VPN 类型                                    | 基于路由                  |
   |                 |                   | SKU                                         | VpnGw1                       |
   |                 |                   | Generation                                  | 第 1 代                  |
   |                 |                   | 虚拟网络                             | ManufacturingVnet            |
   |                 |                   | 子网                                      | GatewaySubnet (10.30.0.0/27) |
   |                 | 公共 IP 地址 | 公共 IP 地址                           | 新建                   |
   |                 |                   | 公共 IP 地址名称                      | ManufacturingVnetGateway-ip  |
   |                 |                   | 公用 IP 地址 SKU                       | 基本                        |
   |                 |                   | 启用主动-主动模式                   | 已禁用                     |
   |                 |                   | 配置 BGP                               | 已禁用                     |
   | 查看 + 创建 |                   | 检查设置，然后选择“创建”。 |                              |
   
   > [!NOTE]
   >
   > 最多需要 45 分钟就可创建虚拟网络网关。 

 

## <a name="task-8-connect-coreservicesvnet-to-manufacturingvnet"></a>任务 8：将 CoreServicesVnet 连接到 ManufacturingVnet 

1. 在“搜索资源、服务和文档(G+/)”中，输入“虚拟网关”，然后从结果中选择“虚拟网关”。

2. 在“虚拟网关”中，选择“CoreServicesVnetGateway”。

3. 在“CoreServicesGateway”中，选择“连接”，然后选择“+ 添加”。

   > [!NOTE]
   >
   >  在完全部署虚拟网关后，才能完成此配置。

4. 使用下表中的信息创建连接：

   | **选项**                     | **值**                         |
   | ------------------------------ | --------------------------------- |
   | 名称                           | CoreServicesGW-ManufacturingGW |
   | 连接类型                | VNet 到 VNet                      |
   | 第一个虚拟网关  | CoreServicesVnetGateway           |
   | 第二个虚拟网关 | ManufacturingVnetGateway          |
   | 共享密钥 (PSK)               | abc123                            |
   | 使用 Azure 专用 IP 地址   | 未选定                      |
   | 启用 BGP                     | 未选定                      |
   | IKE 协议                   | IKEv2                             |
   | 订阅                   | 无需更改               |
   | 资源组                 | 无需更改               |
   | 位置                       | 美国东部                           |

5. 若要创建连接，请选择“确定”。
   

## <a name="task-9-connect-manufacturingvnet-to-coreservicesvnet"></a>任务 9：将 ManufacturingVnet 连接到 CoreServicesVnet

1. 在“搜索资源、服务和文档(G+/)”中，输入“虚拟网关”，然后从结果中选择“虚拟网关”。

2. 在"虚拟网关"中，选择“ManufacturingVnetGateway”。

3. 在“CoreServicesGateway”中，选择“连接”，然后选择“+ 添加”。

4. 使用下表中的信息创建连接：

   | **选项**                     | **值**                         |
   | ------------------------------ | --------------------------------- |
   | 名称                           | ManufacturingGW-to-CoreServicesGW |
   | 连接类型                | VNet 到 VNet                      |
   | 第一个虚拟网关  | ManufacturingVnetGateway          |
   | 第二个虚拟网关 | CoreServicesVnetGateway           |
   | 共享密钥 (PSK)               | abc123                            |
   | 使用 Azure 专用 IP 地址   | 未选定                      |
   | 启用 BGP                     | 未选定                      |
   | IKE 协议                   | IKEv2                             |
   | 订阅                   | 无需更改               |
   | 资源组                 | 无需更改               |
   | 位置                       | 西欧                       |

5. 若要创建连接，请选择“确定”。

## <a name="task-10-verify-that-the-connections-connect"></a>任务 10：验证连接是否成功 

1. 在“搜索资源、服务和文档(G+/)”中，输入“连接”，然后从结果中选择“连接”。

2. 等待，直到两个连接的状态都为“已连接”。 你可能需要刷新屏幕。 

   ![已成功创建 VPN 网关连接。](../media/connections-status-connected.png)

 

## <a name="task-11-test-the-connection-between-the-vms"></a>任务 11：测试 VM 间的连接

1. 在“ManufacturingTestVM”上，打开 PowerShell。

2. 使用以下命令验证 CoreServicesVnet 上现在是否存在与 CoreServicesTestVM 的连接。 请务必使用 CoreServicesTestVM 的 IPv4 地址。

   ```Powershell
   Test-NetConnection 10.20.20.4 -port 3389
   ```

3. 测试连接应成功，且你将看到如下所示的结果：

   ![Test-NetConnection 成功。](../media/test-connection-succeeded.png)

4. 关闭远程桌面连接。

祝贺你！ 你已使用虚拟网关配置了一个 VNet 到 VNet 连接。
