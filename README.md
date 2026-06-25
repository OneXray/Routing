# OneXray Routing Templates

[简体中文](./readme/README.zh_CN.md) | [Русский](./readme/README.ru_RU.md)

This repository provides Xray Setting JSON templates for OneXray. These files
are not subscription feeds and do not use the removed OneXray App sharing
protocol.

## Notes

1. Add the required GeoData entries before saving a template.
2. The JSON files are Xray Setting templates, not complete Raw Json node
   configs. They do not contain proxy nodes.
3. OneXray injects the selected outbound node as `proxy` when starting VPN.

## Required GeoData

Open OneXray, go to `Core > GeoData > Add`, then add the entries required by
your region.

### Mainland China

| Name | Type | URL |
| --- | --- | --- |
| `EnhancedGeoSite` | `domain` | `https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat` |
| `EnhancedGeoIP` | `ip` | `https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat` |

### Iran

| Name | Type | URL |
| --- | --- | --- |
| `IranGeoSite` | `domain` | `https://github.com/bootmortis/iran-hosted-domains/releases/latest/download/iran.dat` |

### Russia

| Name | Type | URL |
| --- | --- | --- |
| `RussiaGeoSite` | `domain` | `https://raw.githubusercontent.com/runetfreedom/russia-v2ray-rules-dat/release/geosite.dat` |
| `RussiaGeoIP` | `ip` | `https://raw.githubusercontent.com/runetfreedom/russia-v2ray-rules-dat/release/geoip.dat` |

## Import

1. Open the matching template link below and copy the JSON content.
2. In OneXray, go to `Core > Xray Settings`.
3. Tap `Add`, then open `Raw Edit`.
4. Paste the JSON content, save it, then select the new Xray Setting.

Mainland China:

```text
https://github.com/OneXray/Routing/raw/refs/heads/main/cn.json
```

Iran:

```text
https://github.com/OneXray/Routing/raw/refs/heads/main/ir.json
```

Russia:

```text
https://github.com/OneXray/Routing/raw/refs/heads/main/ru.json
```
