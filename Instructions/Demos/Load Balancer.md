---
demo:
  title: 负载均衡器
  module: 'Module 04 - Load balancing non-HTTPS traffic '
---
## 配置 Azure 负载均衡器

在此演示中，我们将了解如何创建公共负载均衡器。 

**参考**：[快速入门：使用 Azure 门户创建公共负载均衡器，以便对 VM 进行负载均衡](https://learn.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)

注意：本演示需要一个具有至少一个子网的虚拟网络。  

**显示门户的帮我选择功能**

1. 访问 Azure 门户。

1. 搜索并选择“负载均衡 - 帮我选择”。

1. 使用向导演练不同的方案。
   
**创建负载均衡器**

1. 在 Azure 门户中继续操作。

1. 搜索并选择“负载均衡器”。 创建负载均衡器。 

1. 在“基本信息”选项卡上，探讨 SKU、类型以及层级   。

1. 在“前端 IP 配置”选项卡上，探讨公共 IP 地址的使用。

1. 在“后端池”选项卡上，通过 IP 地址范围选择虚拟网络。

1. 在“入站规则”选项卡上，创建负载均衡规则”。 探讨协议、端口、运行状况探测和会话持续性等参数   。 


