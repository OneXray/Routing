# OneXray 路由模板

[English](../README.md) | [Русский](./README.ru_RU.md)

本仓库提供 OneXray 可手动导入的 Xray Setting JSON 模板。这些文件不是订阅源，也不再使用已经移除的 OneXray App 专用分享协议。

## 注意

1. 保存模板前，需要先添加模板依赖的 GeoData。
2. JSON 文件是 Xray Setting 模板，不是完整的 Raw Json 节点配置，不包含代理节点。
3. OneXray 启动 VPN 时会将当前选中的节点注入为 `proxy`。

## 需要添加的 GeoData

打开 OneXray，进入 `Core > GeoData > Add`，然后根据所在地区添加对应数据。

### 中国大陆

| Name | Type | URL |
| --- | --- | --- |
| `EnhancedGeoSite` | `domain` | `https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat` |
| `EnhancedGeoIP` | `ip` | `https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat` |

### 伊朗

| Name | Type | URL |
| --- | --- | --- |
| `IranGeoSite` | `domain` | `https://github.com/bootmortis/iran-hosted-domains/releases/latest/download/iran.dat` |

### 俄罗斯

| Name | Type | URL |
| --- | --- | --- |
| `RussiaGeoSite` | `domain` | `https://raw.githubusercontent.com/runetfreedom/russia-v2ray-rules-dat/release/geosite.dat` |
| `RussiaGeoIP` | `ip` | `https://raw.githubusercontent.com/runetfreedom/russia-v2ray-rules-dat/release/geoip.dat` |

## 导入

1. 打开下面对应地区的模板链接，复制 JSON 内容。
2. 在 OneXray 中进入 `Core > Xray Settings`。
3. 点击 `Add`，然后打开 `Raw Edit`。
4. 粘贴 JSON 内容并保存，然后选择新建的 Xray Setting。

中国大陆：

```text
https://github.com/OneXray/Routing/raw/refs/heads/main/cn.json
```

伊朗：

```text
https://github.com/OneXray/Routing/raw/refs/heads/main/ir.json
```

俄罗斯：

```text
https://github.com/OneXray/Routing/raw/refs/heads/main/ru.json
```
