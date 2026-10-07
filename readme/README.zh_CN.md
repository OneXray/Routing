# OneXray 自定义路由

[English](../README.md) | [Русский](./README.ru_RU.md)

本仓库提供适用于新版 OneXray **自定义路由**编辑器的模板，包含本地 DNS 配置。
这些文件不是服务器订阅，也不是完整的 Raw JSON 配置。服务器需要另行添加。

## 模板

| 模板 | 分流行为 | 本地 DNS 地址 |
| --- | --- | --- |
| [中国大陆](../cn.json) | 局域网、Apple 域名、中国大陆域名和 IP 直连；其他流量经过 VPN。 | `223.5.5.5` |
| [伊朗](../ir.json) | 伊朗数据集中的域名、私有 IP 和伊朗 IP 直连；其他流量经过 VPN。 | `5.200.200.200` |
| [俄罗斯](../ru.json) | 俄罗斯封锁名单中的域名和 IP 经过 VPN；其余 IP 直连。 | `9.9.9.9` |

每份模板默认使用一个接入节点，可以在编辑器中调整为两个或三个。
模板不绑定具体服务器，也不配置最终出口。

## 导入

1. 下载[中国大陆](https://raw.githubusercontent.com/YuanDevTeam/Routing/main/cn.json)、
   [伊朗](https://raw.githubusercontent.com/YuanDevTeam/Routing/main/ir.json)或
   [俄罗斯](https://raw.githubusercontent.com/YuanDevTeam/Routing/main/ru.json)的 JSON 文件；
   也可以复制完整的 JSON 内容，不要只复制下载链接。
2. 在**连接**页打开**流量方式**，在**自定义路由**中选择**新建自定义路由**。
3. 在编辑器中选择**导入文件**或**读取剪贴板**。OneXray 会下载模板声明的自定义
   Geodata，因此导入时需要能够访问下载地址。
4. 核对名称、规则、接入节点数量和本地 DNS 地址，点击**保存**。
5. 在**流量方式**中选择保存的路由，选择服务器后连接。

最多保存三份自定义路由，名称不能重复。达到上限时，可以编辑已有路由并导入以替换内容，
或者先删除不需要的路由。请使用自定义路由的导入入口，不要通过服务器或 Raw JSON 入口导入。

## DNS 与匹配行为

- 模板只保存本地／直连 DNS 地址。代理 DNS 由 OneXray 管理，固定为 `8.8.8.8`。
  TUN DNS 是独立设置。
- 本地 DNS 的域名匹配范围由动作是 `direct` 的域名规则自动生成。
  IP、端口和网络条件不会指定本地 DNS；本地 DNS 也不是通用回退服务器。
- 俄罗斯模板没有直连域名规则，因此保存的 `9.9.9.9` 只有在添加此类规则后才会使用。
  域名查询使用代理 DNS，不再沿用旧模板将 Quad9 作为通用回退服务器的行为。
- `IPIfNonMatch` 在域名未命中时，解析为 IP 后重新匹配。俄罗斯模板末尾的直连规则
  覆盖 `0.0.0.0/0` 和 `::/0`，不使用匹配全部 TCP/UDP 的规则，确保解析后仍会检查
  封锁 IP 规则。解析未返回 IP 时，这条最终直连规则不会命中。
- 没有命中任何规则的流量遵循 Xray 默认行为，使用第一个接入出站。
  显式选择 VPN 的规则使用 `proxy` 负载均衡器。同一规则中的不同条件类型是“且”的关系，
  同类条件中的多个值是“或”的关系。

## Geodata 依赖

模板的 `geodata.assets` 已声明以下自定义文件名和 HTTPS 下载地址，无需在导入前手动添加。

| 模板 | 文件 | 来源 |
| --- | --- | --- |
| 中国大陆 | `EnhancedGeoSite.dat` | [Loyalsoldier Geosite](https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat) |
| 中国大陆 | `EnhancedGeoIP.dat` | [Loyalsoldier GeoIP](https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat) |
| 伊朗 | `IranGeoSite.dat` | [Iran hosted domains](https://github.com/bootmortis/iran-hosted-domains/releases/latest/download/iran.dat) |
| 俄罗斯 | `RussiaGeoSite.dat` | [Russia Geosite](https://raw.githubusercontent.com/runetfreedom/russia-v2ray-rules-dat/release/geosite.dat) |
| 俄罗斯 | `RussiaGeoIP.dat` | [Russia GeoIP](https://raw.githubusercontent.com/runetfreedom/russia-v2ray-rules-dat/release/geoip.dat) |

伊朗模板还使用 OneXray 默认 `geoip.dat` 中的 `PRIVATE` 和 `IR`，默认数据不写入依赖清单。
导入遇到同名自定义文件时会报错，不会覆盖。若本地已经安装了对应的数据集，可以从导入清单
中移除这一项，并保留规则引用；否则需要同时修改文件名和所有对应的 `ext:` 引用后再导入。
不要删除其他路由仍在使用的数据集。

成功导入后，OneXray 不会在数据库中的路由 JSON 内保留依赖清单；导出时会重新附上所需
自定义数据集的依赖信息。

## 格式

- `name`：导入／分享时使用的名称，1–32 个字符，不能与已有路由重名。
- `dns.servers`：仅一个对象，只包含 `tag: "app-dns-direct"` 和 `address`。
- `outbounds`：1–3 个空对象，例如 `[{}]` 或 `[{}, {}, {}]`，表示接入节点数量。
- `routing`：固定的 `domainStrategy: "IPIfNonMatch"` 和按顺序匹配的 `rules`。
- `geodata.assets`：可选的导入依赖，每项只包含 `file` 和 `url`。

规则通过 `ruleTag` 命名，只支持 `domain`、`ip`、`port`、`network` 四类条件。
每条规则必须且只能选择一种动作：`balancerTag: "proxy"`、`outboundTag: "direct"`
或 `outboundTag: "block"`。

不要填写实际节点、`direct`／`block` 出站定义、`inboundTag`、`type`、DNS 域名列表／
查询策略或运行时负载均衡器定义。运行依赖由 OneXray 生成，配置通过 libXray 校验。
完整的 Xray 配置请使用 Raw JSON。
