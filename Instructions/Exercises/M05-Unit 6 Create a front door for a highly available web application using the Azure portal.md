---
Exercise:
  title: 模块 05-单元 6 - 使用 Azure 门户为高度可用的 Web 应用程序创建 Front Door
  module: Module - Load balancing HTTP(S) traffic in Azure
ms.openlocfilehash: dbdd8fdccdea3ade0ae2d8f67b5b837a13fb94fa
ms.sourcegitcommit: e98d709ed0f96f3c8e8c4e74c3aea821dff153ca
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2022
ms.locfileid: "147922362"
---
# <a name="m05-unit-6-create-a-front-door-for-a-highly-available-web-application-using-the-azure-portal"></a>模块 05-单元 6 - 使用 Azure 门户为高度可用的 Web 应用程序创建 Front Door

 

在此练习中，你将设置 Azure Front Door 配置，将在不同 Azure 区域运行的 Web 应用程序的两个实例汇集在一起。 此配置将流量定向到运行该应用程序的最近站点。 Azure Front Door 会持续监视该 Web 应用程序。 演示在最近的站点不可用时到下一个可用站点的自动故障转移。 网络配置如下图所示：

![Azure Front Door 的网络配置。](../media/front-door-environment-diagram.png)

通过学习本练习，你将能够：

+ 任务 1：创建 Web 应用的两个实例
+ 任务 2：为应用程序创建 Front Door
+ 任务 3：通过实际操作查看 Azure Front Door
+ 任务 4：清理资源


#### <a name="estimated-time-30-minutes"></a>预计用时：30 分钟

## <a name="task-1-create-two-instances-of-a-web-app"></a>任务 1：创建 Web 应用的两个实例

本练习要求 Web 应用程序的两个实例在不同的 Azure 区域中运行。 这两个 Web 应用程序实例在“主动/主动”模式下运行，因此其中的任何一个实例都可以接收流量。 此配置不同于“主动/备用”配置，在后一种配置中，只有一个实例充当故障转移节点。

1. 在 [https://portal.azure.com](https://portal.azure.com/) 中登录 Azure 门户。

2. 在 Azure 主页上，使用全局搜索类型“WebApp”，然后选择服务下的“应用服务”。 

3. 单击“+ 创建”以创建 Web 应用。

4. 在“创建 Web 应用”页的“基本信息”选项卡上，输入或选择以下信息。

   | **设置**      | 值                                                    |
   | ---------------- | ------------------------------------------------------------ |
   | 订阅     | 选择订阅。                                    |
   | 资源组   | 选择资源组 ContosoResourceGroup               |
   | 名称             | 输入 Web 应用的唯一名称。 本示例使用 WebAppContoso-1。 |
   | 发布          | 选择“代码”。                                             |
   | 运行时堆栈    | 选择“.NET Core 3.1 (LTS)”。                              |
   | 操作系统 | 选择“Windows”。                                          |
   | 区域           | 选择“美国中部”。                                       |
   | Windows 计划     | 选择“新建”，然后在文本框中输入“myAppServicePlanCentralUS”。 |
   | SKU 和大小     | 选择“标准 S1: 总共 100 个 ACU，1.75 GB 内存”。        |

5. 选择“审阅 + 创建”，检查“摘要”，然后选择“创建”。   
   ‎完成部署可能需要几分钟时间。

6. 创建第二个 Web 应用。 在 Azure 门户主页上，搜索“WebApp”。

7. 单击“+ 创建”以创建 Web 应用。

8. 在“创建 Web 应用”页的“基本信息”选项卡上，输入或选择以下信息。

   | **设置**      | 值                                                    |
   | ---------------- | ------------------------------------------------------------ |
   | 订阅     | 选择订阅。                                    |
   | 资源组   | 选择资源组 ContosoResourceGroup               |
   | 名称             | 输入 Web 应用的唯一名称。 本示例使用 WebAppContoso-2。 |
   | 发布          | 选择“代码”。                                             |
   | 运行时堆栈    | 选择“.NET Core 3.1 (LTS)”。                              |
   | 操作系统 | 选择“Windows”。                                          |
   | 区域           | 选择“美国东部”。                                          |
   | Windows 计划     | 选择“新建”，然后在文本框中输入 myAppServicePlanEastUS。 |
   | SKU 和大小     | 选择“标准 S1: 总共 100 个 ACU，1.75 GB 内存”。        |

9. 选择“审阅 + 创建”，检查“摘要”，然后选择“创建”。   
   ‎完成部署可能需要几分钟时间。

## <a name="task-2-create-a-front-door-for-your-application"></a>任务 2：为应用程序创建 Front Door

配置 Azure Front Door，以根据两个 Web 应用服务器之间的最小延迟来定向用户流量。 若要开始，请为 Azure Front Door 添加前端主机。

1. 在任何 Azure 门户页上，在“搜索资源、服务和文档 (G+/)”中搜索 Front Door 和 CDN 配置文件，然后选择“Front Door 和 CDN 配置文件” 。

1. 选择“创建 Front Door 和 CDN 配置文件”。 在“比较产品/服务”页上选择“快速创建”。  然后选择“继续创建 Front Door”。

1. 在“基本”选项卡上，输入或选择以下信息。


   | **设置**             | 值                                    |
   | ----------------------- | -------------------------------------------- |
   | 订阅            | 选择订阅。                    |
   | 资源组          | 选择 ContosoResourceGroup                  |
   | 资源组位置 | 接受默认设置                       |
   | 名称                    | 在此订阅中输入唯一名称，例如 FrontDoor(你的姓名首字母)   |
   | 层                    | 标准   |
   | 终结点名称           | FDendpoint   |
   | 原点类型             | 应用服务| 
   | 源主机名        | 之前部署的 Web 应用的名称 |
   

1. 选择“查看并创建”，然后选择“创建” 。

1. 等待资源部署，然后选择“转到资源”。
2. 在“概述”边栏选项卡中的 Front Door 资源上，找到“源组”，选择创建的源组
3. 若要更新源组，请从列表中选择名称“default-origin-group”。 选择“添加源”并添加第二个 Web 应用。 选择“添加”，然后选择“更新”。 

## <a name="task-3-view-azure-front-door-in-action"></a>任务 3：通过实际操作查看 Azure Front Door

创建 Front Door 后，需要花费几分钟时间来全局部署配置。 完成后，访问你创建的前端主机。 

1. 在“概述”边栏选项卡中的 Front Door 资源上，找到为终结点创建的终结点主机名。 该名称应该为 fdendpoint，后跟连字符和随机字符串。 例如 fdendpoint-fxa8c8hddhhgcrb9.z01.azurefd.net。 单击“复制”复制此 FQDN。

1. 在新浏览器选项卡中，导航到 Front Door 终结点 FQDN。 系统将显示默认应用服务页。
   ![显示应用服务信息页的浏览器](../media/app-service-info-page.png)

5. 若要实际测试即时全局故障转移，请尝试以下步骤：

6. 切换到 Azure 门户，搜索并选择“应用服务”。 

7. 选择一个 Web 应用，然后选择“停止”，然后选择“是”进行验证。

   ![显示已停止的 Web 应用的 Azure 门户](../media/stop-web-app.png)

8. 切换回浏览器，然后选择“刷新”。 应会看到相同的信息页。

Web 应用停止时可能有延迟。如果在浏览器中出现错误页面，请刷新页面。

1. 切换回 Azure 门户，找到另一个 Web 应用，然后将其停止。

2. 切换回浏览器，然后选择“刷新”。 此时，应会看到一条错误消息。

   ![显示应用服务错误页的浏览器](../media/web-apps-both-stopped.png)

   祝贺你！ 你已配置并测试了 Azure Front Door。
   

## <a name="task-4-clean-up-resources"></a>任务 4：清理资源
   
   >**注意**：记得删除所有不再使用的新建 Azure 资源。 删除未使用的资源可确保不会出现意外费用。

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```powershell

   Remove-AzResourceGroup -Name 'ContosoResourceGroup' -Force -AsJob

   ```

    >**注意**：该命令以异步方式执行（由 -AsJob 参数决定），因此，虽然你可以随后立即在同一个 PowerShell 会话中运行另一个 PowerShell 命令，但需要几分钟才能实际删除资源组。
