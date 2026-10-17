# BydNavi

Короткая https-страница: из обычной ссылки открывает маршрут в **Яндекс Навигаторе** (и при желании в других навигаторах). Так маршрутом можно делиться в Telegram — мессенджер делает кликабельными только https-ссылки, а `yandexnavi://` из чата не открывается.

## Пример

https://us-kavalchuk.github.io/BydNavi/?lat=53.86&lon=27.45&name=Test

Страница сразу открывает Яндекс Навигатор:

```
yandexnavi://build_route_on_map?lat_to=53.86&lon_to=27.45
```

На Android автопереход и жёлтая кнопка используют intent с пакетом `ru.yandex.yandexnavi`:

```
intent://build_route_on_map?lat_to=53.86&lon_to=27.45#Intent;scheme=yandexnavi;package=ru.yandex.yandexnavi;end
```

На странице также есть:

- запасная ссылка на маршрут в Яндекс Картах (браузер);
- кнопка «Скопировать координаты»;
- блок **Другие навигаторы** (2ГИС, Google Maps, Waze, Apple Maps, HERE WeGo, Sygic, Organic Maps, OsmAnd, `geo:`).

## Параметры

| Параметр | Описание |
| --- | --- |
| `lat`, `lon` | Широта и долгота назначения, оба обязательны |
| `name` | Подпись места на странице |
| `lat_from`, `lon_from` | Точка старта (нужны оба). Без них — от текущего места |
| `app` | Какой навигатор открыть сразу. По умолчанию `yandex` |

Старт и финиш:

https://us-kavalchuk.github.io/BydNavi/?lat=53.86&lon=27.45&lat_from=53.90&lon_from=27.56&name=Test

Сразу открыть другой навигатор (жёлтая кнопка всё равно ведёт в Яндекс Навигатор):

https://us-kavalchuk.github.io/BydNavi/?lat=53.86&lon=27.45&name=Test&app=google

Значения `app`: `yandex` (по умолчанию), `google`, `waze`, `2gis`, `apple`, `here`, `sygic`, `organic`, `osmand`, `geo`.

## Форматы ссылок других навигаторов

Координаты назначения: `lat=53.86`, `lon=27.45`.

| Приложение | URL |
| --- | --- |
| 2ГИС (https, предпочтительно) | `https://2gis.ru/routeSearch/rsType/car/to/27.45,53.86` |
| 2ГИС (схема приложения) | `dgis://2gis.ru/routeSearch/rsType/car/to/27.45,53.86` |
| Google Maps | `https://www.google.com/maps/dir/?api=1&destination=53.86,27.45&travelmode=driving` |
| Waze | `https://waze.com/ul?ll=53.86,27.45&navigate=yes` |
| Apple Maps | `https://maps.apple.com/?daddr=53.86,27.45&dirflg=d` |
| HERE WeGo | `https://share.here.com/r/mylocation/53.86,27.45?m=d` |
| Sygic | `https://go.sygic.com/directions?to=53.86,27.45` |
| Organic Maps | `om://v2/nav?origin=currentLocation&destination=53.86,27.45&mode=drive` |
| OsmAnd | `https://osmand.net/map/?pin=53.86,27.45#14/53.86/27.45` |
| Android geo: | `geo:53.86,27.45` |

У 2ГИС в пути координаты идут как **долгота,широта**. С `lat_from`/`lon_from` в ссылку добавляется отрезок `from/...`.

Источники схем: [Яндекс Навигатор](https://yandex.ru/dev/navigator/doc/ru/concepts/navigator-url-params), [2ГИС](https://help.2gis.ru/question/razrabotchikam-zapusk-ideystviya-vmobilnom-prilozhenii-cherez-deeplink), [Google Maps URLs](https://developers.google.com/maps/documentation/urls/get-started), [Waze Deep Links](https://developers.google.com/waze/deeplinks), [Apple Map Links](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html), [HERE share route](https://docs.here.com/wego-pro/docs/share-route-dl), [Sygic go.sygic.com](https://www.sygic.com/developers/professional-navigation-sdk/ios/custom-url), [Organic Maps API](https://github.com/organicmaps/api-ios), [OsmAnd map URLs](https://osmand.net/docs/user/plan-route/web/).

Яндекс для Навигатора 2.40+ просит подпись ссылки ключом доступа (`client` и `signature`). Ключа в этом репозитории нет, поэтому ссылка собирается без подписи.
