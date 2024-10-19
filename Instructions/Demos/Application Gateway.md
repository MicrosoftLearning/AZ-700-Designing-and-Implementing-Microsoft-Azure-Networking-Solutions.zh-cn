---
demo:
  module: Module 05 - Load balancing HTTPS traffic
  title: 应用程序网关（模块 05）
---
## 配置 Azure 应用程序网关

在此演示中，我们将了解如何创建 Azure 应用程序网关。 

**模拟：**[部署 Azure 应用程序网关](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Deploy%20Azure%20Application%20Gateway)

**参考**：[快速入门：使用 Azure 应用程序网关定向 Web 流量 - Azure 门户](https://learn.microsoft.com/azure/application-gateway/quick-create-portal)

**参考**：[使用 Azure 应用程序网关端到端加密网络流量](https://github.com/MicrosoftDocs/mslearn-end-to-end-encryption-with-app-gateway)

**注意**：为了简单起见，请在完成配置时创建新的虚拟网络和子网。 

**创建 Azure 应用程序网关**

1. 访问 Azure 门户。

1. 搜索并选择“Azure 应用程序网关”。

1. 创建新的网关。

1. 在“基本信息”选项卡上，探讨层级、自动缩放以及实例计数   。

1. 在“前端”选项卡上，探讨 IP 地址类型。

1. 在“后端”选项卡上，探讨空后端池的适用情况。

1. 在“配置”选项卡上，探讨传递规则。 与负载均衡器规则进行比较。

1. 说明在创建网关后要添加后端目标并进行测试。 
