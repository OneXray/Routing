# Шаблоны маршрутизации OneXray

[English](../README.md) | [简体中文](./README.zh_CN.md)

Этот репозиторий содержит JSON-шаблоны Xray Setting для OneXray. Эти файлы не
являются подписками и не используют удаленный фирменный протокол общего доступа
OneXray App.

## Примечания

1. Перед сохранением шаблона добавьте необходимые записи GeoData.
2. JSON-файлы являются шаблонами Xray Setting, а не полными Raw Json
   конфигурациями узлов. Они не содержат прокси-узлы.
3. При запуске VPN OneXray подставляет выбранный outbound-узел как `proxy`.

## Необходимые GeoData

Откройте OneXray, перейдите в `Core > GeoData > Add`, затем добавьте записи,
необходимые для вашего региона.

### Материковый Китай

| Name | Type | URL |
| --- | --- | --- |
| `EnhancedGeoSite` | `domain` | `https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat` |
| `EnhancedGeoIP` | `ip` | `https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat` |

### Иран

| Name | Type | URL |
| --- | --- | --- |
| `IranGeoSite` | `domain` | `https://github.com/bootmortis/iran-hosted-domains/releases/latest/download/iran.dat` |

### Россия

| Name | Type | URL |
| --- | --- | --- |
| `RussiaGeoSite` | `domain` | `https://raw.githubusercontent.com/runetfreedom/russia-v2ray-rules-dat/release/geosite.dat` |
| `RussiaGeoIP` | `ip` | `https://raw.githubusercontent.com/runetfreedom/russia-v2ray-rules-dat/release/geoip.dat` |

## Импорт

1. Откройте ссылку на шаблон для нужного региона и скопируйте JSON.
2. В OneXray перейдите в `Core > Xray Settings`.
3. Нажмите `Add`, затем откройте `Raw Edit`.
4. Вставьте JSON, сохраните его и выберите новый Xray Setting.

Материковый Китай:

```text
https://github.com/OneXray/Routing/raw/refs/heads/main/cn.json
```

Иран:

```text
https://github.com/OneXray/Routing/raw/refs/heads/main/ir.json
```

Россия:

```text
https://github.com/OneXray/Routing/raw/refs/heads/main/ru.json
```
