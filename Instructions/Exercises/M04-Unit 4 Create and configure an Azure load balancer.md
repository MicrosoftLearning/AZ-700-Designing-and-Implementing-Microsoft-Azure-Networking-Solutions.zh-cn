---
Exercise:
  title: 模块 04-第 4 单元 创建并配置 Azure 负载均衡器
  module: Module - Load balancing non-HTTP(S) traffic in Azure
ms.openlocfilehash: f88f70aa0a753425a9c93ac37d034d26ea7685d4
ms.sourcegitcommit: 349c82964aa36c0f69cfaf6a0b36ad8bb0017f06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2022
ms.locfileid: "141483587"
---
# <a name="m04-unit-4-create-and-configure-an-azure-load-balancer"></a>模块 04-第 4 单元 创建并配置 Azure 负载均衡器

在本练习中，你将为虚构的 Contoso Ltd 组织创建一个内部负载均衡器。 

创建内部负载均衡器的步骤与你在此模块中学到的创建公共负载均衡器的步骤非常相似。 主要区别在于，使用公共负载均衡器时，前端可通过公共 IP 地址访问，并从位于虚拟网络外部的主机测试连接；而使用内部负载均衡器时，前端是虚拟网络中的专用 IP 地址，并从来自同一网络内的主机测试连接。

下图展示了将在本练习中部署的环境。

![内部标准负载均衡器关系图](../media/exercise-internal-standard-load-balancer-environment-diagram.png)

 
通过学习本练习，你将能够：

+ 任务 1：创建虚拟网络
+ 任务 2：创建后端服务器
+ 任务 3：创建负载均衡器
+ 任务 4：创建负载均衡器资源
+ 任务 5：测试负载均衡器

## <a name="task-1-create-the-virtual-network"></a>任务 1：创建虚拟网络

在本部分，你将创建虚拟网络和子网。
   
1. 登录到 Azure 门户。

2. 在 Azure 门户主页上，导航到“全局搜索”栏，搜索“虚拟网络”，然后在“服务”下选择“虚拟网络”。  ![Azure 门户主页上“全局搜索”栏中有关虚拟网络的结果。](../media/global-search-bar.PNG)

3. 在“虚拟网络”页上选择“创建”。  ![创建虚拟网络向导。](../media/create-virtual-network.png)

4. 在“基本信息”选项卡上，使用下表中的信息创建虚拟网络。

   | **设置**    | 值                                  |
   | -------------- | ------------------------------------------ |
   | 订阅   | 选择订阅                   |
   | 资源组 | 选择“新建”  名称：IntLB-RG |
   | 名称           | IntLB-VNet                             |
   | 区域         | **（美国）美国东部**                           |


5. 单击“下一页: IP 地址”。

6. 在“IP 地址”选项卡上的“IPv4 地址空间”框中，删除默认值并键入“10.1.0.0/16”。

7. 在“IP 地址”选项卡上，选择“+ 添加子网” 。

8. 在“添加子网”窗格中，提供子网名称“myBackendSubnet”，以及子网地址范围“10.1.0.0/24”。

9. 单击“添加”。

10. 单击“添加子网”，然后提供子网名称“myFrontEndSubnet”以及子网地址范围“10.1.2.0/24”  。 单击“添加”

11. 单击“下一页: 安全性”。

12. 在“BastionHost”下，选择“启用”，然后输入下表中的信息。

    | **设置**                       | **值**                                     |
    | --------------------------------- | --------------------------------------------- |
    | Bastion 名称                      | myBastionHost                             |
    | AzureBastionSubnet 地址空间 | **10.1.1.0/24**                               |
    | 公共 IP 地址                 | 选择“新建”  名称：myBastionIP |


13. 单击“查看 + 创建”。

14. 单击“创建”。

## <a name="task-2-create-backend-servers"></a>任务 2：创建后端服务器

在本部分中，你将为负载均衡器的后端池创建三个位于同一可用集的 VM，将这些 VM 添加到后端池，然后在这三个 VM 上安装 IIS 以测试负载均衡器。

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

2. 在 Cloud Shell 窗格的工具栏中，单击“上传/下载文件”图标，在下拉菜单中单击“上传”，将 azuredeploy.json、azuredeploy.parameters.vm1.json、azuredeploy.parameters.vm2.json 和 azuredeploy.parameters.vm3.json 文件逐个上传到 Cloud Shell 主目录中。

3. 部署以下 ARM 模板以创建此练习所需的 VM：

   ```powershell
   $RGName = "IntLB-RG"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.vm1.json
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.vm2.json
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.vm3.json
   ```

创建这三个 VM 可能需要 5-10 分钟。 无需等待此作业完成，即可继续执行下一个任务。

## <a name="task-3-create-the-load-balancer"></a>任务 3：创建负载均衡器

在本部分中，你将创建一个内部标准 SKU 负载均衡器。 在此练习中创建标准 SKU 负载均衡器（而不是基本 SKU 负载均衡器）的原因是，之后的练习需要使用标准 SKU 版本的负载均衡器。

1. 在 Azure 门户主页上，单击“创建资源”。

2. 在页面顶部的搜索框中，键入“负载均衡器”，然后按“Enter”（注意：不要从列表中进行选择）。

3. 在结果页面上，找到并选择“负载均衡器”（名称下显示“Microsoft”和“Azure Service”的那个）。

4. 单击“创建”。

5. 在“基本信息”选项卡上，使用下表中的信息创建负载均衡器。

   | **设置**           | 值                |
   | --------------------- | ------------------------ |
   | 订阅          | 选择订阅 |
   | 资源组        | IntLB-RG             |
   | 名称                  | myIntLoadBalancer    |
   | 区域                | **（美国）美国东部**         |
   | 类型                  | **内部**             |
   | SKU                   | **标准**             |


6. 单击“下一步:前端 IP 配置”。
7. 单击“添加前端 IP”
8. 在“添加前端 IP 地址”边栏选项卡上，输入下表中的信息，并选择“添加” 。
 
   | **设置**     | **值**                |
   | --------------- | ------------------------ |
   | 名称            | LoadBalancerFrontEnd |
   | 虚拟网络 | IntLB-VNet           |
   | 子网          | myFrontEndSubnet     |
   | 分配      | **动态**              |

9. 单击“查看 + 创建”。

10. 单击“创建”。

## <a name="task-4-create-load-balancer-resources"></a>任务 4：创建负载均衡器资源

在本部分中，将为后端地址池配置负载均衡器设置，然后创建运行状况探测，并指定负载均衡器规则。

### <a name="create-a-backend-pool-and-add-vms-to-the-backend-pool"></a>创建后端池，将 VM 添加到后端池

后端地址池包含连接到负载均衡器的虚拟 NIC 的 IP 地址。

1. 在 Azure 门户主页上，单击“所有资源”，然后在资源列表中单击“myIntLoadBalancer”。

2. 在“设置”下，选择“后端池”，然后单击“添加”。

3. 在“添加后端池”页上，输入下表中的信息。

   | **设置**     | **值**            |
   | --------------- | -------------------- |
   | 名称            | myBackendPool    |
   | 虚拟网络 | IntLB-VNet       |


4. 在“虚拟机”下，单击“添加”。

5. 选中所有 3 个 VM（myVM1、myVM2 和 myVM3）的复选框，然后单击“添加”。

6. 单击“添加”。
   ![图片 7](../media/add-vms-backendpool.png)
   

### <a name="create-a-health-probe"></a>创建运行状况探测器

负载均衡器使用运行状况探测器监视应用的状态。 运行状况探测器基于 VM 对运行状况检查的响应，在负载均衡器中添加或删除 VM。 你将在此处创建运行状况探测来监视 VM 的运行状况。

1. 在“设置”下单击“运行状况探测”，然后单击“添加”。   

2. 在“添加运行状况探测”页上，输入下表中的信息。

   | **设置**         | **值**         |
   | ------------------- | ----------------- |
   | 名称                | myHealthProbe |
   | 协议            | **HTTP**          |
   | 端口                | **80**            |
   | 路径                | **/**             |
   | 时间间隔            | **15**            |
   | 不正常阈值 | **2**             |


3. 单击 **添加**。
   ![图片 5](../media/create-healthprobe.png)

 

### <a name="create-a-load-balancer-rule"></a>创建负载均衡器规则

负载均衡器规则用于定义将流量分配给 VM 的方式。 定义传入流量的前端 IP 配置和后端 IP 池以接收流量。 源端口和目标端口在规则中定义。 你将在此处创建负载均衡器规则。

1. 在负载均衡器的“后端池”页的“设置”下，单击“负载均衡规则”，然后单击“添加”。

2. 在“添加负载均衡规则”页上，输入下表中的信息。

   | **设置**            | **值**                |
   | ---------------------- | ------------------------ |
   | 名称                   | myHTTPRule           |
   | IP 版本             | **IPv4**                 |
   | 前端 IP 地址    | LoadBalancerFrontEnd |
   | 协议               | **TCP**                  |
   | 端口                   | **80**                   |
   | 后端端口           | **80**                   |
   | 后端池           | myBackendPool        |
   | 运行状况探测           | myHealthProbe        |
   | 会话暂留    | **无**                 |
   | 空闲超时(分钟) | **15**                   |
   | 浮动 IP            | **已禁用**             |


3. 单击 **添加**。
   ![图片 6](../media/create-loadbalancerrule.png)

 


 

 

## <a name="task-5-test-the-load-balancer"></a>任务 5：测试负载均衡器

在本部分中，你将创建一个测试 VM，然后测试负载均衡器。

### <a name="create-test-vm"></a>创建测试 VM

1. 在 Azure 门户主页上，依次单击“创建资源”、“虚拟”，然后选择“虚拟机”（如果页面上未列出此资源类型，请使用页面顶部的搜索框搜索并选择该资源类型）  。

2. 在“创建虚拟机”页的“基本信息”选项卡上，使用下表中的信息创建第一个 VM。

   | **设置**          | 值                                    |
   | -------------------- | -------------------------------------------- |
   | 订阅         | 选择订阅                     |
   | 资源组       | IntLB-RG                                 |
   | 虚拟机名称 | **myTestVM**                                 |
   | 区域               | **（美国）美国东部**                             |
   | 可用性选项 | **没有所需的基础结构冗余**    |
   | 映像                | Windows Server 2019 Datacenter - Gen 2   |
   | 大小                 | Standard_DS2_v3 - 2 个 vCPU，8 GiB 内存 |
   | 用户名             | TestUser                                 |
   | 密码             | TestPa$$w0rd!                            |
   | 确认密码     | TestPa$$w0rd!                            |


3. 单击“下一页: 磁盘”，然后单击“下一页: 网络”。 

4. 在“网络”选项卡上，使用下表中的信息配置网络设置。

   | **设置**                                                  | **值**                     |
   | ------------------------------------------------------------ | ----------------------------- |
   | 虚拟网络                                              | IntLB-VNet                |
   | 子网                                                       | myBackendSubnet           |
   | 公共 IP                                                    | 更改为“无”            |
   | NIC 网络安全组                                   | **高级**                  |
   | 配置网络安全组                             | 选择现有的“myNSG” |
   | 是否将此虚拟机置于现有负载均衡解决方案之后？ | 关（未选中）           |


5. 单击“查看 + 创建”。

6. 单击“创建”。

7. 等待最后一个 VM 部署完毕，然后再继续执行下一任务。

### <a name="connect-to-the-test-vm-to-test-the-load-balancer"></a>连接到测试 VM 以测试负载均衡器

1. 在 Azure 门户主页上，单击“所有资源”，然后在资源列表中单击“myIntLoadBalancer”。

2. 在“概述”页上，记下“专用 IP 地址”，或将其复制到剪贴板。 注意：可能需要选择“查看更多”才能看到“专用 IP 地址”字段。 

3. 单击“主页”，在 Azure 门户主页上，单击“所有资源”，然后单击刚刚创建的 myTestVM 虚拟机。

4. 在“概述”页上，选择“连接”，然后选择“Bastion”  。

5. 单击“使用 Bastion”。

6. 在“用户名”框中键入“TestUser”，在“密码”框中键入“TestPa$$w0rd!”，然后单击“连接”。 如果弹出窗口阻止程序在阻止新窗口，请允许弹出窗口阻止程序并再次连接。

7. 将在另一个浏览器标签页中打开“myTestVM”窗口。

8. 如果出现“网络”窗格，请单击“是”。

9. 单击任务栏中的“Internet Explorer”图标以打开 Web 浏览器。

10. 在“设置 Internet Explorer 11”对话框中单击“确定”。

11. 将上一步中的“专用 IP 地址”（例如 10.1.0.4）输入（或粘贴）到浏览器的地址栏中，然后按 Enter。

12. IIS Web 服务器的默认 Web 主页将显示在浏览器窗口中。 后端池中的三个虚拟机之一将做出响应。
    ![图片 8](../media/load-balancer-web-test-1.png)

13. 如果多次单击浏览器中的刷新按钮，则将看到响应是从内部负载均衡器后端池中的不同 VM 随机发出的。
    ![图片 9](../media/load-balancer-web-test-2.png)

## <a name="clean-up-resources"></a>清理资源

   >**注意**：记得删除所有不再使用的新建 Azure 资源。 删除未使用的资源可确保不会出现意外费用。

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```powershell
   Remove-AzResourceGroup -Name 'IntLB-RG' -Force -AsJob
   ```

    >**注意**：该命令以异步方式执行（由 -AsJob 参数决定），因此，虽然你可以随后立即在同一个 PowerShell 会话中运行另一个 PowerShell 命令，但需要几分钟才能实际删除资源组。
