---
Exercise:
  title: 模块 05-第 4 单元 部署 Azure 应用程序网关
  module: Module - Load balancing HTTP(S) traffic in Azure
ms.openlocfilehash: bb7e4a9d6c91177afa8b22c127bc56d61f95ebd3
ms.sourcegitcommit: 15778a5942c3177246f4fb1077d4233ddeaf95a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2022
ms.locfileid: "140741993"
---
# <a name="m05-unit-4-deploy-azure-application-gateway"></a>模块 05-第 4 单元 部署 Azure 应用程序网关
 

在本练习中，你将使用 Azure 门户创建一个应用程序网关。 然后对其进行测试以确保其正常运行。

该应用程序网关将应用程序 Web 流量定向到后端池中的特定资源。 你将向端口分配侦听器，创建规则，并向后端池中添加资源。 为简单起见，本文使用了带有公共前端 IP 的简单设置、一个在应用程序网关上托管单个站点的基本侦听器、一个基本的请求路由规则，以及后端池中的两台虚拟机。

Azure 需要一个虚拟网络才能在创建的资源之间通信。 可以创建新的虚拟网络，或者使用现有的虚拟网络。 在此示例中，将在创建应用程序网关的同时创建新的虚拟网络。 在独立的子网中创建应用程序网关实例。 在本示例中创建两个子网：一个用于应用程序网关，另一个用于后端服务器。

通过学习本练习，你将能够：

+ 任务 1：创建应用程序网关
+ 任务 2：创建虚拟机
+ 任务 3：将后端服务器添加到后端池
+ 任务 4：测试应用程序网关


## <a name="task-1-create-an-application-gateway"></a>任务 1：创建应用程序网关

1. 使用 Azure 帐户登录到 [Azure 门户](https://portal.azure.com/)。

2. 在任意 Azure 门户页上的“搜索资源、服务和文档 (G+/)”中，输入“应用程序网关”，然后从结果中选择“应用程序网关” 。
    ![Azure 门户搜索应用程序网关](../media/search-application-gateway.png)    

3. 在“应用程序网关”页上，选择“+ 创建”。

4. 在“创建应用程序网关”的“基本信息”选项卡上，输入或选择以下信息：

   | **设置**         | 值                                    |
   | ------------------- | -------------------------------------------- |
   | 订阅        | 选择订阅。                    |
   | 资源组      | 选择 " 新建 ContosoResourceGroup"       |
   | 应用程序网关 | ContosoAppGateway                            |
   | 区域              | 选择“美国东部”                           |
   | 虚拟网络     | 选择“新建”                        |

5. 在“创建虚拟网络”中，输入或选择以下信息：

   | **设置**       | **值**                          |
   | ----------------- | ---------------------------------- |
   | 名称              | ContosoVNet                        |
   | ADDRESS SPACE |                                    |
   | 地址范围     | 10.0.0.0/16                        |
   | SUBNETS       |                                    |
   | 子网名称       | 将“默认值”更改为“AGSubnet” |
   | 地址范围     | 10.0.0.0/24                        |
   | 子网名称       | BackendSubnet                      |
   | 地址范围     | 10.0.1.0/24                        |

6. 选择“确定”以返回到“创建应用程序网关”的“基本信息”选项卡。

7. 接受其他设置的默认值，然后选择“下一步:  前端”。

8. 在“前端”选项卡上，验证“IP 地址类型”是否设置为“公共”  。

9. 对于“公共 IP 地址”，选择“新增”，为“公共 IP 地址名称”输入“AGPublicIPAddress”，然后选择“确定”。

10. 在完成时选择“下一步:**后端**。

11. 在“后端”选项卡上，选择“添加后端池” 。

12. 在打开的“添加后端池”窗口中，输入以下值以创建空的后端池  ：

    | **设置**                      | **值**   |
    | -------------------------------- | ----------- |
    | 名称                             | BackendPool |
    | 添加没有目标的后端池 | 是         |

13. 在“添加后端池”窗口中，选择“添加”以保存后端池配置并返回到“后端”选项卡  。

14. 在“后端”  选项卡上，选择“下一步:  配置”。

15. 在“配置”选项卡上，将连接使用传递规则创建的前端和后端池。

16. 在“传递规则”列中，选择“添加传递规则”。

17. 在“规则名称”框中，输入“RoutingRule”。

18. 在“侦听器”选项卡上，输入或选择以下信息：

    | **设置**   | **值**         |
    | ------------- | ----------------- |
    | 侦听器名称 | 侦听器          |
    | 前端 IP   | 选择“公共” |

19. 接受“侦听器”选项卡上其他设置的默认值。

    ![在 Azure 门户上添加应用程序网关路由规则](../media/routing-rule-listener-tab.png)

20. 选择“后端目标”选项卡以配置传递规则的其余部分。

21. 在“后端目标”选项卡上，输入或选择以下信息：

    | **设置**   | **值**      |
    | ------------- | -------------- |
    | 目标类型   | 后端池   |
    | HTTP 设置 | **新建** |

22. 在“添加 HTTP 设置”中，输入或选择以下信息：

    | **设置**        | **值**   |
    | ------------------ | ----------- |
    | HTTP 设置名称 | HTTPSetting |
    | 后端端口       | 80          |

23. 接受“添加 HTTP 设置”窗口中其他设置的默认值，然后选择“添加”以返回到“添加传递规则”。

24. 选择“添加”以保存传递规则，然后返回到“配置”选项卡。

25. 在完成时选择“下一步:  标记”，然后选择“下一步:  查看 + 创建”。

26. 查看“审阅 + 创建”选项卡上的设置

27. 选择“创建”以创建虚拟网络、公共 IP 地址和应用程序网关。 

Azure 可能需要数分钟时间来创建应用程序网关。 请等待部署成功完成，然后再前进到下一部分。

## <a name="task-2-create-virtual-machines"></a>任务 2：创建虚拟机

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

2. 在 Cloud Shell 窗格的工具栏中，选择“上传/下载文件”图标，在下拉菜单中选择“上传”，将文件 backend.json 和 backend.parameters.json 从源文件夹 F:\Allfiles\Exercises\M05 逐个上传到 Cloud Shell 主目录  。

3. 部署以下 ARM 模板以创建此练习所需的 VM：

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile backend.json -TemplateParameterFile backend.parameters.json
   ```
  
4. 部署完成后，转到 Azure 门户主页，然后选择“虚拟机”。

5. 验证是否已创建两个虚拟机。

## <a name="task-3-add-backend-servers-to-backend-pool"></a>任务 3：将后端服务器添加到后端池

1. 在 Azure 门户菜单上，选择“所有资源”，或搜索并选择“所有资源”。 然后选择“ContosoAppGateway”。

2. 在“设置”下，选择“后端池”。 

3. 选择“BackendPool”。

4. 在“编辑后端池”页的“后端目标”下，在“目标类型”中，选择“虚拟机”。

5. 在“目标”下，选择“BackendVM1”。 

6. 在“目标类型”中，选择“虚拟机”。

7. 在“目标”下，选择“BackendVM2”。 

   ![在 Azure 门户中，将目标后端添加到后端池](../media/edit-backend-pool.png)

8. 选择“保存”。

等待部署完成之后再继续下一步。

## <a name="task-4-test-the-application-gateway"></a>任务 4：测试应用程序网关

虽然不需 IIS 即可创建应用程序网关，但本练习中安装了它，用来验证 Azure 是否已成功创建应用程序网关。

### <a name="use-iis-to-test-the-application-gateway"></a>使用 IIS 测试应用程序网关：

1. 在“概览”页上找到应用程序网关的公共 IP 地址。 

   ![在 Azure 门户中，查询前端公共 IP 地址 ](../media/app-gw-public-ip.png)

2. 复制公共 IP 地址，然后将其粘贴到浏览器的地址栏中，以便浏览该 IP 地址。

3. 检查响应。 有效响应验证应用程序网关是否已成功创建，以及是否能够成功连接后端。

   ![浏览器 - 显示 BackendVM1 或 BackendVM2，具体取决于响应请求的后端服务器。](../media/browse-to-backend.png)

4. 多次刷新浏览器就会看到与 BackendVM1 和 BackendVM2 的连接。

祝贺你！ 你已配置并测试了 Azure 应用程序网关。
