---
demo:
  title: 'VNET 对等互连            '
  module: Module 01 - Introduction to Azure Virtual Networks
---
## 配置 VNet 对等互连

注意：在本演示中，你将需要两个虚拟网络。 

参考：[使用 VNet 对等互连连接虚拟网络 - 教程](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**配置第一个虚拟网络的 VNet 对等互连**

1. 在 Azure 门户中，选择第一个虚拟网络 **** 。 查看对等互连的值。 

1. 在“设置”下，选择“对等互连”和“+ 添加新对等互连” ****  **** 。

1. 配置对等互连第二个虚拟网络。 使用信息图标查看不同的设置。 

1. 对等互连完成后，查看对等互连状态。 

**确认第二个虚拟网络的 VNet 对等互连**

1. 在 Azure 门户中，选择第二个虚拟网络 ****

1. 在“设置”下，选择“对等互连” ****  **** 。

1. 可以看到，一个对等互连已自动创建。 请注意，“对等互连状态”为“已连接” ****   **** 。

1. 在实验室中，学生将创建对等互连并测试虚拟机之间的连接。 
