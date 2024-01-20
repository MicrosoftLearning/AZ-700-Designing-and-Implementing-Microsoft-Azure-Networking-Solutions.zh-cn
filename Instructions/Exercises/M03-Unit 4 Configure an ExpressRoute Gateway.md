---
Exercise:
  title: 模块 03 - 第 4 单元 配置 ExpressRoute 网关
  module: Module 03 - Design and implement Azure ExpressRoute
---
# 模块 03-第 4 单元 配置 ExpressRoute 网关

## 部署 ExpressRoute 网关

![虚拟网络网关图示。](../media/4-exercise-configure-expressroute-gateway.png)

若要通过 ExpressRoute 连接 Azure 虚拟网络和本地网络，必须首先创建虚拟网络网关。 虚拟网络网关有两个用途：在网络之间交换 IP 路由和路由网络流量。 

**注意：** 我们提供 **[交互式实验室模拟](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Configure%20an%20ExpressRoute%20gateway)** ，让你能以自己的节奏点击浏览实验室。 你可能会发现交互式模拟与托管实验室之间存在细微差异，但演示的核心概念和思想是相同的。

#### 预计用时：60 分钟（包括约 45 分钟的部署等待时间）

网关类型

创建虚拟网络网关时，需要指定几项设置。 其中一个必要设置“-GatewayType”指定是否将网关用于 ExpressRoute 或 VPN 流量。 两种网关类型是：

- **VPN** - 若要通过公共 Internet 发送加密流量，请使用网关类型“VPN”。 这也称为 VPN 网关。 站点到站点连接、点到站点连接和 VNet 到 VNet 连接都使用 VPN 网关。
- **ExpressRoute** - 若要在专用连接上发送网络流量，请使用网关类型“ExpressRoute”。 这也称为 ExpressRoute 网关，是配置 ExpressRoute 时使用的网关类型。

对于每种网关类型，每个虚拟网络只能有一个虚拟网络网关。 例如，一个虚拟网络网关使用 -GatewayType VPN，另一个使用 -GatewayType ExpressRoute。


通过学习本练习，你将能够：

+ 任务 1：创建 VNet 和网关子网
+ 任务 2：创建虚拟网络网关。



## 任务 1：创建 VNet 和网关子网

1. 在任意 Azure 门户页上的“搜索资源、服务和文档”中，输入“虚拟网络”，然后从结果中选择“虚拟网络” 。

1. 在“虚拟网络”页上选择“+ 创建”。

1. 在“创建虚拟网络”窗格上的“基本信息”选项卡上，使用下表中的信息创建 VNet：

   | 设置          | 值                        |
   | -------------------- | -------------------------------- |
   | 虚拟网络名称 | CoreServicesVnet                 |
   | 资源组       | ContosoResourceGroup             |
   | 位置             | 美国东部                          |

1. 选择“下一步: IP 地址”。

1. 在“IP 地址”选项卡上的“IPv4 地址空间”中，输入 10.20.0.0/16，然后选择“+ 添加子网”。 

1. 在“添加子网”窗格中，使用下表中的信息创建子网：

   | 设置                  | 值     |
   | ---------------------------- | ------------- |
   | 网关子网名称          | GatewaySubnet |
   | 网关子网地址空间 | 10.20.0.0/27  |

1. 然后选择“添加”。 

1. 在“创建虚拟网络”页面上，选择“审阅 + 创建”。

   ![Azure 门户 - 添加网关子网](../media/add-gateway-subnet.png)

1. 确认 VNet 通过验证，然后选择“创建”。

> [!Note]  
>
> 如果使用的是双堆栈虚拟网络，并计划通过 ExpressRoute 使用基于 IPv6 的专用对等互连，请选择“添加 IPv6 地址空间”并输入 IPv6 地址范围值。

## 任务 2：创建虚拟网络网关。

1. 在任意 Azure 门户页上的“搜索资源、服务和文档 (G+/)”中，输入“虚拟网络网关”，然后从结果中选择“虚拟网络网关” 。

1. 在“虚拟网络网关”页上，选择“+ 创建”。

1. 在“创建虚拟网络网关”页上，使用下表中的信息创建网关：

   | 设置               | **值**                  |
   | ------------------------- | -------------------------- |
   | **项目详细信息**       |                            |
   | 资源组            | ContosoResourceGroup       |
   | **实例详细信息**      |                            |
   | 名称                      | CoreServicesVnetGateway    |
   | 区域                    | 美国东部                    |
   | 网关类型              | ExpressRoute               |
   | SKU                       | 标准                   |
   | 虚拟网络           | CoreServicesVnet           |
   | **公共 IP 地址**     |                            |
   | 公共 IP 地址         | 新建                 |
   | 公共 IP 地址名称    | CoreServicesVnetGateway-IP |
   | 公用 IP 地址 SKU     | 基本                      |
   | 分配                | 不可配置           |
   
1. 选择“查看 + 创建”  。

1. 确认网关配置通过验证，然后选择“创建”。

1. 完成部署后，选择“转到资源”。

> [!Note] 
>
> 部署网关可能需要 45 分钟。

祝贺你！ 已成功创建虚拟网络、网关子网和 ExpressRoute 网关。

