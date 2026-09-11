# OneXray Custom Routing

[简体中文](./readme/README.zh_CN.md) | [Русский](./readme/README.ru_RU.md)

Importable templates for OneXray's current **Custom Routing** editor, including
its Local DNS setting. These are not server subscriptions or complete Raw JSON
configurations. Add your own servers separately.

## Templates

| Template | Routing behavior | Local DNS address |
| --- | --- | --- |
| [Mainland China](./cn.json) | Private networks, Apple domains and mainland China domains/IPs connect directly; other traffic uses the VPN. | `223.5.5.5` |
| [Iran](./ir.json) | Domains in the Iran dataset, private IPs and Iranian IPs connect directly; other traffic uses the VPN. | `5.200.200.200` |
| [Russia](./ru.json) | Domains/IPs on the Russian blocking lists use the VPN; remaining IPs connect directly. | `9.9.9.9` |

Each template requests one entry node. Change the entry count in the editor to
use two or three. The templates do not select servers or configure a final exit.

## Import

1. Download the JSON for [Mainland China](https://raw.githubusercontent.com/OneXray/Routing/main/cn.json),
   [Iran](https://raw.githubusercontent.com/OneXray/Routing/main/ir.json) or
   [Russia](https://raw.githubusercontent.com/OneXray/Routing/main/ru.json).
   Alternatively, copy the complete JSON content, not the download URL.
2. On **Connect**, open **Traffic method** and select **New custom route** under
   **Custom Routing**.
3. Choose **Import file** or **Read clipboard** in the editor. OneXray downloads
   the custom Geodata declared by the template; internet access is needed.
4. Review the name, rules, entry count and Local DNS address, then **Save**.
5. Select the saved route in **Traffic method**, choose your servers and connect.

OneXray supports up to three custom routes with unique names. At the limit,
import into an existing route to replace its content or delete an unused route
first. Do not use the server or Raw JSON import flow for these templates.

## DNS and matching

- Only the local/direct DNS address is stored. Proxy DNS remains `8.8.8.8`,
  managed by OneXray. TUN DNS settings are independent.
- Local DNS domain matches are derived from rules whose action is `direct`.
  IP, port and network conditions do not select a local DNS resolver. The local
  server is not a general fallback.
- Russia has no direct-domain rule, so its saved `9.9.9.9` address is not used
  until you add one. Domain lookups use proxy DNS; the old general Quad9
  fallback is no longer part of this template.
- `IPIfNonMatch` resolves an unmatched domain and checks the rules again using
  the resulting IPs. Russia's last direct rule covers `0.0.0.0/0` and `::/0`,
  rather than all TCP/UDP traffic, so blocked IPs can still be checked after
  resolution. If resolution yields no IP, this last rule cannot match.
- Unmatched traffic follows Xray's default: the first entry outbound. Explicit
  VPN rules use the `proxy` balancer. Different condition types in one rule are
  ANDed; values within one type are alternatives.

## Geodata dependencies

Custom filenames and HTTPS download URLs are included in `geodata.assets`.
Do not add these datasets manually before importing.

| Template | File | Source |
| --- | --- | --- |
| Mainland China | `EnhancedGeoSite.dat` | [Loyalsoldier Geosite](https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat) |
| Mainland China | `EnhancedGeoIP.dat` | [Loyalsoldier GeoIP](https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat) |
| Iran | `IranGeoSite.dat` | [Iran hosted domains](https://github.com/bootmortis/iran-hosted-domains/releases/latest/download/iran.dat) |
| Russia | `RussiaGeoSite.dat` | [Russia Geosite](https://raw.githubusercontent.com/runetfreedom/russia-v2ray-rules-dat/release/geosite.dat) |
| Russia | `RussiaGeoIP.dat` | [Russia GeoIP](https://raw.githubusercontent.com/runetfreedom/russia-v2ray-rules-dat/release/geoip.dat) |

Iran also uses OneXray's default `geoip.dat` for `PRIVATE` and `IR`; default data
is not included in the manifest. A custom filename collision is rejected, not
overwritten. To reuse an installed matching dataset, remove that asset from the
import manifest and keep its rule references unchanged. Otherwise, rename both
the asset and all corresponding `ext:` references before importing. Do not
delete datasets needed by another route.

After import, OneXray removes the manifest from stored routing JSON and includes
the required custom dependencies again when exporting.

## Format

- `name`: a unique display name of 1–32 characters in shared/imported JSON.
- `dns.servers`: one object with `tag: "app-dns-direct"` and `address` only.
- `outbounds`: one to three empty objects, such as `[{}]` or `[{}, {}, {}]`.
- `routing`: `domainStrategy: "IPIfNonMatch"` and ordered `rules`.
- `geodata.assets`: optional import dependencies, each with `file` and `url`.

Rules use `ruleTag` for their name and support `domain`, `ip`, `port` and
`network`. Each rule selects exactly one action: `balancerTag: "proxy"`,
`outboundTag: "direct"` or `outboundTag: "block"`.

Do not include real nodes, `direct`/`block` outbound definitions, `inboundTag`,
`type`, DNS domain lists/query strategies, or runtime balancer definitions.
OneXray generates runtime dependencies and validates the configuration through
libXray. Full Xray configurations belong in Raw JSON instead.
