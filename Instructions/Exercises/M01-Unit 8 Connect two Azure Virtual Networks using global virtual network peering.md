---
Exercise:
  title: 模块 01 - 第 8 单元 使用全局虚拟网络对等互连连接两个 Azure 虚拟网络
  module: Module 01 - Introduction to Azure Virtual Networks
---
# <a name="m01-unit-8-connect-two-azure-virtual-networks-using-global-virtual-network-peering"></a>模块 01 第 8 单元 - 使用全局虚拟网络对等互连连接两个 Azure 虚拟网络

## <a name="exercise-scenario"></a>练习场景 
在本单元中，你将通过添加对等互连来允许通信流，以配置 CoreServicesVnet 和 ManufacturingVnet 之间的连接。 

在本单元中，你将学习以下内容：

+ 任务 1：创建虚拟机以测试配置
+ 任务 2：使用 RDP 连接到测试 VM
+ 任务 3：测试 VM 间的连接
+ 任务 4：在 CoreServicesVnet 和 ManufacturingVnet 之间创建 VNet 对等互连
+ 任务 5：测试 VM 间的连接
+ 任务 6：清理资源

#### <a name="estimated-time-20-minutes"></a>预计用时：20 分钟

## <a name="task-1-create-a-virtual-machine-to-test-the-configuration"></a>任务 1：创建虚拟机以测试配置

在本部分中，你将在 Manufacturing VNet 上创建一个测试 VM，用来测试是否可以从 ManufacturingVnet 访问另一个 Azure 虚拟网络中的资源。

### <a name="create-manufacturingvm"></a>创建 ManufacturingVM

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

2. 在 Cloud Shell 窗格的工具栏中，选择“上传/下载文件”图标，在下拉菜单中选择“上传”，将文件 ManufacturingVMazuredeploy.json 和 ManufacturingVMazuredeploy.parameters.json 从源文件夹 F:\Allfiles\Exercises\M01 逐个上传到 Cloud Shell 主目录  。

3. 部署以下 ARM 模板以创建此练习所需的 VM：

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
  
4. 部署完成后，转到 Azure 门户主页，然后选择“虚拟机”。

5. 验证是否已创建虚拟机。

## <a name="task-2-connect-to-the-test-vms-using-rdp"></a>任务 2：使用 RDP 连接到测试 VM

1. 在 Azure 门户主页上，选择“虚拟机”。

2. 选择“ManufacturingVM”。

3. 在“ManufacturingVM”中，选择“连接”&gt;“RDP”。

4. 在“ManufacturingVM | 连接”中，选择“下载 RDP 文件”。

5. 将 RDP 文件保存到桌面。

6. 使用 RDP 文件连接到 ManufacturingVM，用户名为 TestUser，密码为 TestPa$$w0rd! 。

7. 在 Azure 门户主页上，选择“虚拟机”。

8. 选择“TestVM1”。

9. 在 TestVM1 中，选择“连接”&gt;“RDP”。

10. 在“TestVM1 | 连接”中，选择“下载 RDP 文件”。

11. 将 RDP 文件保存到桌面。

12. 使用 RDP 文件连接 TestVM1，用户名为 TestUser，密码为 TestPa$$w0rd! 。

13. 在这两个 VM 上的“选择设备的隐私设置”中，选择“接受”。

14. 在这两个 VM 上的“网络”中，选择“是”。

15. 在 TestVM1 上，打开 PowerShell 提示符，然后运行以下命令：ipconfig

16. 记录 IPv4 地址。 

 

## <a name="task-3-test-the-connection-between-the-vms"></a>任务 3：测试 VM 间的连接

1. 在“ManufacturingVM”上，打开 PowerShell 提示符。

2. 使用以下命令验证 CoreServicesVnet 上是否不存在与 TestVM1 的连接。 请确保对 TestVM1 使用 IPv4 地址。

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```


3. 测试连接应失败，且你将看到如下所示的结果：![Test-NetConnection 10.20.20.4 - 端口 3389 显示失败的 PowerShell 窗口](../media/test-netconnection-fail.png)

 

## <a name="task-4-create-vnet-peerings-between-coreservicesvnet-and-manufacturingvnet"></a>任务 4：在 CoreServicesVnet 和 ManufacturingVnet 之间创建 VNet 对等互连

1. 在 Azure 主页上，选择“虚拟网络”，然后选择“CoreServicesVnet”。

2. 在“CoreServicesVnet”中的“设置”下，选择“对等互连”。
   ![CoreServicesVnet 对等互连设置的屏幕截图](../media/create-peering-on-coreservicesvnet.png)

3. 在“CoreServicesVnet”|“对等互连”上，选择“+ 添加”。

4. 使用下表中的信息创建对等互连。

| **节**                          | **选项**                                    | 值                             |
| ------------------------------------ | --------------------------------------------- | ------------------------------------- |
| 此虚拟网络                 |                                               |                                       |
|                                      | 对等互连链接名称                             | 将 CoreServicesVnet 连接到 ManufacturingVnet |
|                                      | 到远程虚拟网络的流量             | 允许（默认）                       |
|                                      | 从远程虚拟网络转接的流量 | 允许（默认）                       |
|                                      | 虚拟网络网关或路由服务器       | “无”（默认）                        |
| 远程虚拟网络               |                                               |                                       |
|                                      | 对等互连链接名称                             | 将 ManufacturingVnet 连接到 CoreServicesVnet |
|                                      | 虚拟网络部署模型              | 资源管理器                      |
|                                      | 我知道我的资源 ID                         | 未选定                          |
|                                      | 订阅                                  | MOC 订阅-lodxxxxxxxx          |
|                                      | 虚拟网络                               | ManufacturingVnet                     |
|                                      | 到远程虚拟网络的流量             | 允许（默认）                       |
|                                      | 从远程虚拟网络转接的流量 | 允许（默认）                       |
|                                      | 虚拟网络网关或路由服务器       | “无”（默认）                        |
| 检查设置，然后选择“添加”。 |                                               |                                       |
|                                      |                                               |                                       |

 >**注意**：如果你没有“MOC 订阅”，请使用之前一直使用的订阅。 这只是一个名称而已。

5. 在“CoreServicesVnet”|“对等互连”中，验证是否列出了“将 CoreServicesVnet 连接到 ManufacturingVnet”对等互连。

6. 在“虚拟网络”下，选择“ManufacturingVnet”，然后验证是否列出了“将 ManufacturingVnet 连接到 CoreServicesVnet”对等互连。

 

## <a name="task-5-test-the-connection-between-the-vms"></a>任务 5：测试 VM 间的连接

1. 在“ManufacturingVM”上，打开 PowerShell 提示符。

2. 使用以下命令验证 CoreServicesVnet 上现是否存在与 TestVM1 的连接。 

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```


3. 测试连接应成功，且你将看到如下所示的结果：![Test-NetConnection 10.20.20.4 - 端口 3389 显示 TCP 测试成功: true 的 Powershell 窗口](../media/test-connection-succeeded.png)

 

祝贺你！ 通过添加对等互连，你已成功配置 VNet 之间的连接。 

## <a name="task-6-clean-up-resources"></a>任务 6：清理资源

   >**注意**：记得删除所有不再使用的新建 Azure 资源。 删除未使用的资源可确保不会出现意外费用。

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。 （如有必要，使用默认设置创建 Cloud Shell 存储。）

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```powershell
   Remove-AzResourceGroup -Name 'ContosoResourceGroup' -Force -AsJob
   ```

    >**注意**：该命令以异步方式执行（由 -AsJob 参数决定），因此，虽然你可以随后立即在同一个 PowerShell 会话中运行另一个 PowerShell 命令，但需要几分钟才能实际删除资源组。
