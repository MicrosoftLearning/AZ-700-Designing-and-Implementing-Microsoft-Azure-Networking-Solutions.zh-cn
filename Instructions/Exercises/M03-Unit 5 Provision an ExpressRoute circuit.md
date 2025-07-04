---
Exercise:
  title: 模块 03 - 第 5 单元 预配 ExpressRoute 线路
  module: Module 03 - Design and implement Azure ExpressRoute
---
# 模块 03-第 5 单元 预配 ExpressRoute 线路

## 练习场景

在本练习中，你将使用 Azure 门户和 Azure 资源管理器部署模型创建 ExpressRoute 线路。

### 交互式实验室模拟

>**备注**：之前提供的实验室模拟环境已停止使用。

### 预计用时：15 分钟

![用于练习的 ExpressRoute 线路布局示意图](../media/5-exercise-provision-expressroute-circuit.png)

### 工作技能

通过学习本练习，你将能够：

+ 任务 1：创建和预配 ExpressRoute 线路
+ 任务 2：检索服务密钥
+ 任务 3：取消预配 ExpressRoute 线路


## 任务 1：创建和预配 ExpressRoute 线路

1. 从浏览器导航到 [Azure 门户](https://portal.azure.com/)并使用 Azure 帐户登录。

   >**重要说明**：从发布服务密钥之时起将对 ExpressRoute 线路进行计费。 确保连接服务提供商准备好预配线路后就执行此操作。

1. 在Azure 门户菜单中，搜索并选择 “**ExpressRoute 电路**”。

1. 在**创建 ExpressRoute** 页面上，将**资源组**指定为 `ExpressRouteResourceGroup`。 然后选择“**标准复原能力**”以获取“**复原能力**”

1. 对于***线路详细信息**，请确保指定正确的区域（**美国东部 2**）、线路名称（**TestERCircuit**）、对等互连位置（**西雅图**）、提供程序（**Equinix**）、带宽（**50Mbps**）、SKU 层（**标准**）和数据计量计费模型（**按流量计费**）。

1. 选择“查看 + 创建”  。

1. 确认 ExpressRoute 配置通过验证，然后选择“创建”。

![Azure 门户 - 创建 ExpressRoute 配置选项卡](../media/expressroute-create-configuration2.png)

+ 端口类型确定你是连接到服务提供商还是直接在对等位置连接到 Microsoft 的全球网络。
+ 新建或从经典线路中导入确定是要创建新线路还是要将经典线路迁移到 Azure 资源管理器。
+ 提供商指你将向其请求提供服务的 Internet 服务提供商。
+ “对等互连位置”  是与 Microsoft 建立对等互连的实际位置。

> [!Important]
>
> “对等互连位置”指明了与 Microsoft 建立对等互连的[实际位置](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-locations)。 此位置与“Location”属性 没有 关系，后者指的是 Azure 网络资源提供商所在的地理位置。 尽管两者之间没有关系，但最好是选择地理上与线路对等互连位置靠近的网络资源提供商。

+ **SKU** 确定是启用 ExpressRoute 本地版、ExpressRoute 标准版还是 ExpressRoute 高级版加载项。 可以指定“本地”以获取本地 SKU，指定“标准”以获取标准 SKU，或指定“高级”以获取高级版加载项  。 可以更改 SKU 以启用高级版加载项。

   >**重要说明**：不能将 SKU 从“标准”/“高级”更改为“本地”。

+ **计费模型**确定计费类型。 可以指定“Metered”**** 以获取数据流量套餐，指定“Unlimited”**** 以获取无限制流量套餐。 可以将计费类型从“按流量计费”更改为“不限流量” 。

> [!Important]
>
> 无法将类型从“不限流量”更改为“按流量计费”。

+ **允许经典操作**将允许经典虚拟网络链接到线路。

## 任务 2：检索服务密钥

1. 可以通过选择“所有服务”&gt;“网络”&gt;“ExpressRoute 线路”，来查看创建的所有线路。

   ![Azure 门户 - 创建 ExpressRoute 资源菜单](../media/expressroute-circuit-menu.png)

1. 在订阅中创建的所有 ExpressRoute 线路都将在此处显示。

   ![Azure 门户 - 显示现有 Expressroute 线路](../media/expressroute-circuit-list.png)

1. “线路”页显示线路的属性。 服务密匙字段中显示服务密匙。 服务提供商将需要“服务密钥”来完成预配过程。 服务密钥为线路专属。 **必须将服务密钥发送给连接服务提供商进行预配。**

   ![Azure 门户 - ExpressRoute 线路属性显示服务密钥](../media/expressroute-circuit-overview.png)

1. 在此页上，“提供商状态”指明了服务提供商端的当前预配状态。 “线路状态”指明了 Microsoft 端的状态。

1. 创建新的 ExpressRoute 线路时，线路将为以下状态：

   + 提供程序状态：未预配
   + 线路状态：已启用

   + 当连接服务提供商正在为你启用线路时，线路将更改为以下状态：
     + 提供商状态：正在预配
     + 线路状态：已启用
   + 若要使用 ExpressRoute 线路，该线路必须处于以下状态：
     + 提供商状态：已预配
     + 线路状态：已启用
   + 应定期检查预配状态和线路状态。

![Azure 门户 - 现已预配显示状态的 ExpressRoute 线路属性](../media/provisioned.png)

祝贺你！ 已经创建了一个 ExpressRoute 线路，并找到了该服务密钥，这需要完成对线路的预配。

## 任务 3：取消预配 ExpressRoute 线路

如果 ExpressRoute 线路服务提供商预配状态为“正在预配”或“已预配”，则必须与服务提供商合作，在他们那一端取消预配线路。 在服务提供商取消对线路的预配并向我们发送通知之前，Microsoft 将继续保留资源并向你收费。

   >**备注**：在取消预配前，必须取消所有虚拟网络与 ExpressRoute 线路的链接。 如果此操作失败，请检查是否有虚拟网络链接到了该线路。 如果服务提供商已取消预配线路（服务提供商预配状态设置为“未预配”），则可以删除线路。 这样就会停止对线路的计费。

## 清理资源

可以选择“删除”图标来删除 ExpressRoute 线路。 请先确保提供程序状态为“未预配”，然后再继续操作。

![Azure 门户 - 删除 ExpressRoute 线路](../media/expressroute-circuit-delete.png)

   >**注意**：记得删除所有不再使用的新建 Azure 资源。 删除未使用的资源可确保不会出现意外费用。

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话 。

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```powershell
   Remove-AzResourceGroup -Name 'ContosoResourceGroup' -Force -AsJob
   Remove-AzResourceGroup -Name 'ExpressRouteResourceGroup' -Force -AsJob
   ```

   >**注意**：该命令以异步方式执行（由 -AsJob 参数决定），因此，虽然你可以随后立即在同一个 PowerShell 会话中运行另一个 PowerShell 命令，但需要几分钟才能实际删除资源组。

## 使用 Copilot 扩展学习

Copilot 可帮助你了解如何使用 Azure 脚本工具。 Copilot 还可以帮助了解实验室中未涵盖的领域或需要更多信息的领域。 打开 Edge 浏览器并选择“Copilot”（右上角）或导航到*copilot.microsoft.com*。 花几分钟时间尝试这些提示。
+ 哪些服务提供商可以提供 Azure ExpressRoute？
+ Azure ExpressRoute 最常见的配置问题是什么？ 出现这个问题该怎么办？

## 通过自定进度的培训了解详细信息

+ [Azure ExpressRoute 简介](https://learn.microsoft.com/training/modules/intro-to-azure-expressroute/)。 在本模块中，了解 Azure ExpressRoute 是什么及其提供的功能。
+ [设计和实现 ExpressRoute](https://learn.microsoft.com/training/modules/design-implement-azure-expressroute/)。 在本模块中，了解如何设计和实现 Azure ExpressRoute、ExpressRoute Global Reach 和 ExpressRoute FastPath。

## 关键结论

恭喜你完成本实验室的内容。 下面是本实验室的主要重点。 
+ 借助 Azure ExpressRoute，组织可以将其本地网络直接连接到 Microsoft Azure 和 Microsoft 365 云。 Azure ExpressRoute 使用由 Microsoft 合作伙伴提供的专用高带宽连接。
+ Microsoft 保证 ExpressRoute 专用连接的可用性至少为 99.95%。 连接是专用的，通过专线传输，第三方无法拦截流量。
+ 在本地网络和 Microsoft 云之间创建连接时，可以采用四种不同的方式：CloudExchange 归置、点到点以太网连接、任意位置之间的 (IPVPN) 连接 和 ExpressRoute Direct。
+ ExpressRoute 功能由 SKU 决定：本地版、标准版和高级版。 
