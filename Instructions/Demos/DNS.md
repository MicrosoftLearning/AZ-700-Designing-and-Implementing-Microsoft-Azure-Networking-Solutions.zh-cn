---
demo:
  module: Module 01 - Introduction to Azure Virtual Networks
  title: DNS 名称解析（模块 01）
---
## 配置 Azure DNS

在本演示中，你将探索 Azure DNS。

**参考：**[模拟：在 Azure 中配置 DNS 设置](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Configure%20DNS%20settings%20in%20Azure)

参考：[教程：托管域和子域 - Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)

**创建 DNS 区域**

1. 访问 Azure 门户。

1. 搜索 DNS 区域服务 ****  。

1. 创建 DNS 区域并说明该区域的用途。 对于名称，可以使用 contoso.internal.com。

1.  请等待 DNS 区域创建完毕。 可能需要刷新页面。 ****  

**添加 DNS 记录集**

参考：[教程：创建别名记录以引用区域资源记录](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. 创建区域后，选择“+记录集” **** 。

1. 使用“类型”下拉列表查看各种记录类型。 ****   查看如何使用不同的记录类型。 请注意当你选择不同记录类型时记录信息如何变化。

1. 创建 A 记录作为示例。 

