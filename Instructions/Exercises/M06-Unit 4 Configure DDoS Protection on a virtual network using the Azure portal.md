---
Exercise:
  title: 模块 06-单元 4 使用 Azure 门户在虚拟网络上配置 DDoS 防护
  module: Module - Design and implement network security
ms.openlocfilehash: a3d6752859f81544082d6aa40ca0d455d217d8f4
ms.sourcegitcommit: e98d709ed0f96f3c8e8c4e74c3aea821dff153ca
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2022
ms.locfileid: "147922338"
---
# <a name="m06-unit-4-configure-ddos-protection-on-a-virtual-network-using-the-azure-portal"></a>模块 06-单元 4 使用 Azure 门户在虚拟网络上配置 DDoS 防护

作为 Contoso 网络安全团队的负责人，你将在虚拟网络上运行模拟 DDoS 攻击。 以下步骤将引导你完成创建虚拟网络、配置 DDoS 防护以及创建攻击，你可以借助遥测和指标的帮助进行观察和监视。

通过学习本练习，你将能够：

+ 任务 1：创建资源组
+ 任务 2：创建 DDoS 防护计划
+ 任务 3：在新的虚拟网络上启用 DDoS 防护
+ 任务 4：配置 DDoS 遥测
+ 任务 5：配置 DDoS 诊断日志
+ 任务 6：配置 DDoS 警报
+ 任务 7：提交 DDoS 服务请求以运行 DDoS 攻击
+ 任务 8：清理资源



#### <a name="estimated-time-40-minutes"></a>估计时间：40 分钟

## <a name="task-1-create-a-resource-group"></a>任务 1：创建资源组

1. 登录到 Azure 帐户。

2. 在 Azure 门户主页上，选择“资源组”。

3. 选择“创建”。 

4. 在“基本信息”选项卡上的“资源组”中，输入“MyResourceGroup”。

5. 对于“区域”，请选择“美国东部”。

6. 选择“查看 + 创建”。

7. 选择“创建”。



## <a name="task-2-create-a-ddos-protection-plan"></a>任务 2：创建 DDoS 防护计划

1. 在 Azure 门户主页上，在“搜索”框中键入 DDoS，并在出现“DDoS 防护计划”时选择它 。

2. 选择“+ 新建”。 

3. 在“基本信息”选项卡的“资源组”列表中，选择刚创建的资源组。

4. 在“实例名称”框中，键入“MyDdoSProtectionPlan”，然后单击“查看 + 创建”  。

5. 选择“创建”。

 

## <a name="task-3-enable-ddos-protection-on-a-new-virtual-network"></a>任务 3：在新的虚拟网络上启用 DDoS 防护

在此，你将在新的虚拟网络而不是现有虚拟网络上启用 DDoS，因此，首先需要创建新的虚拟网络，然后使用你之前创建的计划对其启用 DDoS 防护。

1. 在 Azure 门户主页上，选择“创建资源”，然后在搜索框中键入“虚拟网络”，并在出现“虚拟网络”时选择它  。

2. 在“虚拟网络”页上选择“创建”。  

3. 在“基本信息”选项卡上，选择先前创建的资源组。

4. 在“名称”框中，键入“MyVirtualNetwork”，然后单击“安全性”选项卡。 

5. 在“安全性”选项卡的“DDoS 防护标准”旁边，选择“启用”。

6. 在“DDoS 防护计划”下拉列表中，选择“MyDdosProtectionPlan”。

   ![“创建虚拟网络”-“安全性”选项卡](../media/create-virtual-network-security-for-ddos-protection.png)

7. 单击“查看 + 创建”。

8. 单击“创建”。

 

## <a name="task-4-configure-ddos-telemetry"></a>任务 4：配置 DDoS 遥测

创建公共 IP 地址，然后在后续步骤中设置遥测。

1. 在 Azure 门户主页上，选择“创建资源”，然后在搜索框中键入“公共 IP”，然后在出现“公共 IP 地址”时单击它。

2. 在“公共 IP 地址”页上，单击“创建”。

3. 在“创建公共 IP 地址”页的“SKU”下，选择“基本”。

4. 在“名称”框中，键入“MyPublicIPAddress”。

5. 在“IP 地址分配”下，选择“静态”。

6. 在“DNS 名称标签”中，输入“mypublicdnsxx”（其中 xx 是使此名称唯一的缩写）。

7. 从列表中选择你的资源组。

   ![创建公共 IP 地址](../media/create-public-ip-address-for-ddos-telemetry.png)

8. 单击“**创建**”。

9. 在 Azure 主页上，单击“所有资源”。

10. 在资源列表中，单击“MyDdosProtectionPlan”。

11. 在“监视”下，选择“指标”。 

12. 选择“范围”框，并选中“MyPublicIPAddress”旁的复选框。

    ![为 DDoS 遥测创建指标范围](../media/create-metrics-scope-for-ddos-telemetry.png)

13. 单击“应用” 。

14. 在“指标”框中，选择“删除入站数据包的 DDoS”。

15. 在“聚合”框中，选择“最大值”。

    ![为 DDoS 遥测创建的指标](../media/metrics-created-for-ddos-telemetry.png)

 

## <a name="task-5-configure-ddos-diagnostic-logs"></a>任务 5：配置 DDoS 诊断日志

1. 在 Azure 主页上，单击“所有资源”。

2. 在资源列表中，单击“MyPublicIPAddress”。

3. 在“监视”下，选择“诊断设置”   。

4. 单击“添加诊断设置”。 

5. 在“诊断设置”页上的“诊断设置名称”框中，键入“MyDiagnosticSetting”。 

6. 在“类别详细信息”下，选中所有 3 个“日志”复选框和“AllMetrics”复选框。

7. 在“目标详细信息”下，选中“发送到 Log Analytics 工作区”复选框。 此时，你可以选择预先存在的 Log Analytics 工作区，但由于尚未为诊断日志设置目标，因此你只需输入设置，然后在本练习的下一步中将其丢弃。

   ![为 DDoS 配置新的诊断设置](../media/configure-ddos-diagnostic-settings-new.png)

8. 正常情况下，现在可以单击“保存”以保存诊断设置。 请注意，此选项仍灰显，因为我们尚未完成设置配置。

9. 单击“放弃”，然后单击“是”。

 

## <a name="task-6-configure-ddos-alerts"></a>任务 6：配置 DDoS 警报

在此步骤中，将创建一个虚拟机，为其分配一个公共 IP 地址，然后配置 DDoS 警报。

### <a name="create-the-vm"></a>创建 VM

1. 在 Azure 门户主页上，选择“创建资源”，然后在搜索框中键入“虚拟机”，并在出现“虚拟机”时单击它。

2. 在“虚拟机”页上，单击“创建”。

3. 在“基本信息”选项卡上，使用下表中的信息创建新 VM。

   | **设置**           | 值                                                    |
   | --------------------- | ------------------------------------------------------------ |
   | 订阅          | 选择订阅                                     |
   | 资源组        | MyResourceGroup                                          |
   | 虚拟机名称  | MyVirtualMachine                                         |
   | 区域                | 你的区域                                                  |
   | 可用性选项  | 不需要基础结构冗余                   |
   | 映像                 | Ubuntu Server 18.04 LTS - 第 1 代（如果需要，请选择“配置 VM 生成链接”） |                     
   | 大小                  | 选择“查看所有大小”，然后在列表中选择“B1ls”并选择“选择”“(Standard_B1ls - 1 vcpu，0.5 GiB内存)”    |
   | 身份验证类型   | **SSH 公钥**                                           |
   | 用户名              | **azureuser**                                                |
   | SSH 公钥源 | 生成新密钥对                                    |
   | 密钥对名称         | myvirtualmachine-ssh-key                                 |


4. 单击“查看 + 创建”。

5. 单击“创建”。

6. 在“生成新密钥对”对话框中，单击“下载私钥并创建资源”。

7. 保存私钥。

8. 部署完成后，单击“转到资源”。

### <a name="assign-the-public-ip-address"></a>分配公共 IP 地址

1. 在新虚拟机的“概述”页上的“设置”下，单击“网络”。

2. 在“网络接口”旁边，单击“myvirtualmachine-nic” 。 NIC 的名称可能不同。

3. 在“设置”下，单击“IP 配置”。

4. 选择“ipconfig1”。

5. 在“公共 IP 地址”列表中，选择“MyPublicIPAddress”。

6. 单击“保存”  。

   ![更改 DDoS VM 的公共 IP 地址](../media/change-public-ip-config-for-ddos-vm-new.png)

### <a name="configure-ddos-alerts"></a>配置 DDoS 警报

1. 在 Azure 主页上，单击“所有资源”。

2. 在资源列表中，单击“MyDdosProtectionPlan”。

3. 在“监视”下，选择“警报” 。

4. 单击“新建警报规则”。

5. 在“创建警报规则”页的“范围”下，单击“编辑资源”。

6. 在“选择资源”窗格的“按资源类型”筛选框中，向下滚动列表并选择“公共 IP 地址”。

   ![新的警报规则将范围更改为公共 IP 地址](../media/new-alert-rule-change-scope-to-public-ip-address-1.png)

7. 在“资源”列表中，选择“MyPublicIPAddress”，然后单击“完成”。

8. 在“创建警报规则”页的“条件”下，单击“添加条件”。

9. 选择“是否受到 DDoS 攻击”。

   ![向警报规则添加条件 - 选择信号](../media/add-condition-to-alert-rule-1.png)

10. 在“运算符”框中，选择“大于或等于”。

11. 在“阈值”中，输入“1”（表示受攻击）。

12. 单击“完成”。

    ![向警报规则添加条件 - 配置信号逻辑](../media/add-condition-to-alert-rule-2.png)

13. 返回到“创建警报规则”页，在“警报规则详细信息”部分的“警报规则名称”中，输入“MyDdosAlert”   。

    ![创建新警报规则的终点](../media/new-alert-rule-end.png)

14. 单击“创建警报规则”。

 

## <a name="task-7-submit-a-ddos-service-request-to-run-a-ddos-attack"></a>任务 7：提交 DDoS 服务请求以运行 DDoS 攻击

1. 使用 [BreakingPoint Cloud](https://breakingpoint.cloud/) 创建帐户

2. 根据以下屏幕截图中的设置设置 DDoS 测试（可能需要使用试用帐户选择 100k pps 测试大小），但在“目标 IP 地址”框中指定自己的“MyPublicIPAddress”资源的 IP 地址（例如 51.140.137.219）![DDOSAttack](https://user-images.githubusercontent.com/46939028/138599420-58bef33a-2597-4fa2-919f-bf1614037bc3.JPG)  

   ![DDoS 测试安装](../media/ddos-test-setup.png)

3. 在 Azure 门户主页上，单击“所有资源”。

4. 在资源列表中，单击“MyPublicIPAddress”资源，然后在“监视”下单击“指标”。 

5. 在“指标”框中，从列表中选择“是否受到 DDoS 攻击”。

6. 可在此处看到发生 DDoS 攻击。 请注意，可能需要整整 10 分钟才能看到结果。

   ![显示受 DDoS 攻击的资源的指标](../media/metrics-showing-resource-under-attack.png)

 
## <a name="task-8-clean-up-resources"></a>任务 8：清理资源

>**注意**：记得删除所有不再使用的新建 Azure 资源。 删除未使用的资源可确保不会出现意外费用。

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```powershell
   Remove-AzResourceGroup -Name 'MyResourceGroup' -Force -AsJob
   ```

    >**注意**：该命令以异步方式执行（由 -AsJob 参数决定），因此，虽然你可以随后立即在同一个 PowerShell 会话中运行另一个 PowerShell 命令，但需要几分钟才能实际删除资源组。
