NAT 边界防火墙开关支持基于内网资产进行流量管控与安全防护，同时支持基于 SNAT、DNAT 进行的网络流量转发。
## 操作指南

1. 登录 [云防火墙控制台](https://console.cloud.tencent.com/cfw)，在左侧导航栏中，选择【防火墙开关】>【NAT边界开关】，进入 NAT 边界开关页面。
>?当某个 NAT 边界防火墙开关开启后，对应子网的互联网流量将经过防火墙，届时访问控制规则、入侵防御功能将对其生效，流量日志也会生成。
2. 在 “NAT 边界开关”页面，可进行创建实例、同步资产、查看并监控基于 NAT 边界的带宽情况等操作。


###  **创建实例**	
1. 在 [NAT 边界开关页面](https://console.cloud.tencent.com/cfw/switch/nat) 下，单击【创建实例】。
	![](https://main.qcloudimg.com/raw/c59558e085b6e3b7da47870f588caea7.png)
2. 在“新建 NAT 边界防火墙”弹窗中，可为当前账号创建一个新的 NAT 边界防火墙实例，填写相关字段，单击【下一步】。
	>?创建“NAT 边界防火墙”实例，涉及大量后台配置工作，这个步骤可能需要持续若干分钟。
	>
	![](https://main.qcloudimg.com/raw/dada8612c7f4278e4c1415fbb17929dd.png)
	**字段说明：**
	- **地域**：选择创建地域，支持国内所有地域，创建实例后不可更改。
	>?用户可在拥有 VPC 的所有国内地域（支持香港地域）中进行地域选择，但已经创建了 NAT 边界防火墙的地域将无法选择。
	- **可选区**：根据需求选合适的择可用区。
	- **实例名称**：输入实例名称。
	- **带宽规格**：根据需求选择带宽规格，最小20Mbps，最大60Mbps，如需更多带宽请 [升级扩容](https://buy.cloud.tencent.com/cfw)。
	>?互联网带宽保持一致，如果分了多个 NAT 防火墙，那么多个 NAT 防火墙的带宽之和，要小于等于互联网边界的带宽。
	- **模式**：分为新增模式和接入模式。
		- **新增模式**：若当前地域没有 NAT 网关，新增模式可以通过 NAT 边界防火墙内置的 NAT 功能，实现指定实例通过防火墙访问互联网。
		- **接入模式**：若当前地域已有 NAT 网关，或者希望公网对外的出口 IP 保持不变，接入模式可以将 NAT 边界防火墙平滑接入到 NAT 网关与 CVM 实例之间。
	- **弹性 IP**：若选择新建弹性 IP，系统会自动为用户申请一个弹性 IP，用户也可从所有闲置的弹性 IP 中选择一个进行绑定。
3. 选择需要接入的 VPC 或 NAT，单击【创建】,即可创建成功。

### 防火墙开关
在 [防火墙开关页面](https://console.cloud.tencent.com/cfw/switch/nat?tab=switch)，支持开启或关闭 NAT 边界防护。云防火墙会定时自动同步云资产，因此不用担心资产变更后的防火墙配置（例如，变更了某个子网，防火墙会在短时间内自动同步）。
- **开启防护**
	>!开启开关后，请勿在 [私有网络控制台](https://console.cloud.tencent.com/vpc/vpc?rid=1) 中手动变更开关对应的路由，否则将导致防火墙丢失路由而引发网络中断。
	>
	- **方式1**：在实例列表上方，单击【开启防护】，所有未开启的 NAT 边界防火墙开关将被打开，所有路由表将会自动添加下一跳类型为 NAT 边界防火墙的路由策略，所有子网的互联网流量将会经过 NAT 边界防火墙。
		>?若用户选择开启同一路由表关联的所有子网，系统会自动在该路由表中增加一条下一跳指向 NAT 边界防火墙的路由策略，并关闭原访问公网的路由策略，因此该路由表关联的所有子网的互联网流量，将会经过 NAT 边界防火墙。
		>
		![](https://main.qcloudimg.com/raw/f5a6cb3777494e6c6f7faa5a2ec3cb74.png)
	- **方式2**：单独开启防火墙开关。
		- **新增模式**：如想开启对某个子网实例的 NAT 边界防护，可以在右侧操作栏，单击“防火墙开关”按钮，开启防火墙开关后，将开启对该子网资产的防护。
>?若用户仅选择开启某个子网，系统会自动为当前子网创建新的路由表，复制现有的全部路由策略，在新路由表中增加一条下一跳指向 NAT 边界防火墙的路由策略，并关闭原访问公网的路由策略，因此该子网的互联网流量，将会经过 NAT 边界防火墙。
>
![](https://main.qcloudimg.com/raw/dbad366888b468b30205e5ae1ab95036.png)
		- **接入模式**：一个防火墙开关对应一个子网，用来控制流量是否经过 NAT 边界防火墙，关联了同一个路由表的子网将会同时开启或关闭，创建 NAT 边界防火墙（接入模式）后，系统会自动开启防火墙开关，以保证网络不中断。
		>?开启开关后，系统会自动修改子网关联路由表的路由策略，以及子网对应的端口转发规则，该子网的流量牵引至 NAT 边界防火墙。
		>
![](https://main.qcloudimg.com/raw/8df50a9eced91ff15eff901c3f7421a6.png)
- **关闭防护**
	- **方式1**：在实例列表上方，单击【关闭防护】，所有已开启的 NAT 边界防火墙开关将被关闭，NAT 边界防火墙会自动关闭下一跳类型为 NAT 边界防火墙的所有路由表的路由策略，所有子网将断开与互联网的连接，用户需要在 [私有网络控制台](https://console.cloud.tencent.com/vpc/vpc?rid=1) 手动启动新的路由策略。
 >?若用户选择关闭同一路由表关联的所有子网，系统会自动关闭该路由表中下一跳指向 NAT 边界防火墙的路由策略，该路由表关联的所有子网将会断开与互联网的连接。
 	>
![](https://main.qcloudimg.com/raw/e4aef85f8fec9e5d9a8e7afae6e0d9aa.png)
 		- **方式2**：单独关闭防火墙开关。
			- **新增模式**：如想关闭对某个子网实例的NAT边界防护，可以在防火墙操作栏，单击“防火墙开关”按钮，关闭对该子网资产的防护，关闭开关后，用户需要前往 [私有网络控制台](https://console.cloud.tencent.com/vpc/vpc?rid=1) 手动启用新的路由策略。
 >?若用户仅选择关闭某个子网，系统会自动为当前子网创建新的路由表，复制现有的全部路由策略，并关闭下一跳指向 NAT 边界防火墙的路由策略，该子网将会断开与互联网的连接。
 >
![](https://main.qcloudimg.com/raw/b0a7d83c39201adfca9d215dfb6093ab.png)
			- **接入模式**：接入模式下，如想单独关闭防火墙开关，可以在防火墙操作栏，单击“防火墙开关”按钮，接入模式关联了同一个路由表的子网将会同时关闭。
			>?关闭开关后，系统会自动恢复子网关联路由表的路由策略，以及子网子网对应的端口转发规则，该子网的流量将恢复原先路径，不会经过 NAT 边界防火墙。
			>
![](https://main.qcloudimg.com/raw/25de90f0acc96be53dc16393b0ee49d2.png)

### 实例配置
- **端口转发**
在 [实例配置页面](https://console.cloud.tencent.com/cfw/switch/nat?tab=config) ，可以查看用户基于 NAT 边界防火墙实例所添加的 DNAT 端口转发规则，以及与实例关联的弹性 IP。
>?
>- 接入模式中，NAT 边界防火墙会自动同步现有NAT网关的端口转发规则，从而保证流量通行，后续对于该规则的操作，请在 [云防火墙控制台](https://console.cloud.tencent.com/cfw/ac/nat) 中进行。
>- 开启防火墙开关的子网 SNAT、DNAT 流量都会经过防火墙，关闭开关的子网 SNAT、DNAT 流量都走原先路径。
>- 请勿前往私有网络控制台操作端口转发规则，否则可能造成网络中断。
>
	1. 在实例配置页面的端口转发页签下，单击【新建规则】。
![](https://main.qcloudimg.com/raw/a49df48278081ba2968e45e6f3d2e4c4.png)
	2. 在“新建端口转发规则”弹框中，用户可为当前 NAT 边界防火墙实例添加一条外部 IP 为用户所绑定的弹性 IP 的 DNAT 规则。
	>?
	>- 在外部 IP 端口下拉框内，提供的选项为当前 NAT 边界防火墙实例所绑定的弹性 IP。
	>- 输入内部 IP 地址时，用户需填写本地域 VPC 网段内可用的 IP。
	>
![](https://main.qcloudimg.com/raw/ca4969cb2bd037ebc8c5193a85118c3d.png)
- **出口绑定**
在新增模式下，当规则列表为空时，所有 VPC 的子网将随机选择 NAT 网关访问互联网。
>!接入模式暂不支持出口绑定。
>
	1. 在实例配置页面的出口绑定页签下，单击【新建规则】。
![](https://main.qcloudimg.com/raw/1735cd0543b9359f5b4581e3d44cf7a1.png)
	2. 在“新建出口绑定规则”弹框中，提供防火墙实例 ID 信息，用户可为当前 NAT 边界防火墙添加 SNAT 规则。
	>?协议可选子网和私有网络，VPC 或子网的选项选择接入 NAT 边界防火墙，且当前没有绑定出口 NAT 规则的 VPC 或子网。
	>
![](https://main.qcloudimg.com/raw/2adb73cc8dd1129ff18701ea3627a0e4.png)
- **域名解析**
	- 域名解析开关对应整个 NAT 边界防火墙实例，用来控制 DNS 流量是否经过 NAT 边界防火墙，默认关闭，接入 DNS 流量开关生效于所有纳入 NAT 防火墙管控的 VPC。
![](https://main.qcloudimg.com/raw/472403c3a06a5f9a35d2c865d366dd0a.png)
		- 开启开关后，系统会修改所接入 VPC 的 DNS 解析地址，将 DNS 流量牵引至 NAT 边界防火墙，从而获取全流量域名。
	>?接入 VPC 中若存在未开启防火墙开关的子网，可能导致该子网 DNS 解析产生明显延迟，建议开启全部防火墙开关后再启用此开关。
	>
		- 关闭开关后，系统会恢复所接入 VPC 的 DNS 解析地址，DNS 流量将恢复原先路径，不再经过 NAT 边界防火墙。
	- **应用场景**：NAT 防火墙支持将用户的 DNS 解析地址改为 NAT 防火墙的 IP，从而将用户的 DNS 流量牵引至防火墙，防火墙继续请求真实 DNS 解析服务器，并返回 DNS 响应给指定的服务器，不区分 NAT 防火墙的模式。
		1. 在 [NAT 边界规则](https://console.cloud.tencent.com/cfw/ac/nat) 页面，单击【出向规则】。
		2. 在出向规则页签中，单击【添加规则】。
		3. 在添加规则页面，填写相关字段，并选择 DNS 协议。
		![](https://main.qcloudimg.com/raw/247aebda68b11b47b32707fb8bee1017.png)
- **关联弹性 IP**
	1. 在实例配置页面右侧的“关联弹性 IP”模块，单击【+绑定弹性IP】。
	2. 在单选下拉框内，用户可为当前 NAT 边界防火墙实例绑定一个系统新建的弹性 IP，或在当前地域拥有的所有闲置弹性 IP 里选择一个进行绑定。
	>?
	>- 关联弹性 IP 功能目前只支持新增模式，暂未支持迁移模式。
	>- 解除绑定某个弹性 IP 时，页面上与其相应的 DNAT 规则也会消失。
	>
![](https://main.qcloudimg.com/raw/48461beaa0162204aa2db464d949458a.png)


## 相关信息
- 如需对所持有的公网 IP 以及关联的云上资产，配置对应的防火墙开关，可参见 [互联网边界防火墙开关](https://cloud.tencent.com/document/product/1132/46928) 进行操作。
- 如需自动探测 VPC 信息与互通关系，并在每一对互通的 VPC 间，建立云防火墙开关，可参见 [VPC 间防火墙开关](https://cloud.tencent.com/document/product/1132/46930) 进行操作。
- 已绑定外网 IP 的主机，如需直接通过外网 IP 访问，请参见 [调整 NAT 网关和 EIP 的优先级](https://cloud.tencent.com/document/product/552/30012) 文档。
- 如遇到 NAT 边界防火墙相关问题，可参见 [防火墙相关](https://cloud.tencent.com/document/product/1132/54852#nat-.E8.BE.B9.E7.95.8C.E9.98.B2.E7.81.AB.E5.A2.99) 文档。
