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

На странице:

1. Жёлтая кнопка — **Яндекс Навигатор** (как автопереход).
2. Вторая кнопка — **Waze**.
3. «Скопировать координаты».
4. Блок **Другие навигаторы**: 2GIS, Google Maps, Apple Maps, HERE WeGo, Sygic, Organic Maps, OsmAnd, `geo:` (+ запасной `dgis://`). Waze в этом списке не дублируется.

Яндекс Карты с страницы убраны.

### Android / iOS: открытие приложения, не сайта

Обычный `https://waze.com/ul…` из Telegram/Chrome часто остаётся в браузере. Поэтому:

- **Android Waze** (кнопка и `?app=waze`):  
  `intent://ul?ll=LAT,LON&navigate=yes#Intent;scheme=waze;package=com.waze;S.browser_fallback_url=<https waze.com/ul…>;end`
- **iOS Waze**: `waze://?ll=LAT,LON&navigate=yes` (иначе https).
- То же для других приложений с известным пакетом: Google Maps (`google.navigation` / `com.google.android.apps.maps`), 2GIS (`dgis` / `ru.dublgis.dgismobile`), HERE (`here.directions` / `com.here.app.maps`), Sygic (`com.sygic.aura`), Organic Maps (`om` / `app.organicmaps`), OsmAnd (`https` + `net.osmand`).

### Telegram Mini App (`web_app` кнопка)

Страница подключает `https://telegram.org/js/telegram-web-app.js` и, если есть `Telegram.WebApp.initData` (настоящий Mini App):

1. `ready()` + `expand()`;
2. **без** автоперехода на `intent://` (в WebView Mini App он часто ничего не делает);
3. по нажатию кнопки / Main Button: сначала `location.href` на intent/схему, затем `Telegram.WebApp.openLink(https…, { try_browser: 'chrome' })` — Telegram разрешает в `openLink` только http(s), поэтому для intent открывается **эта же страница** во внешнем Chrome, где срабатывает обычный автопереход, и Mini App закрывается (`close()`).

Обычные url-кнопки, браузер и ГУ не меняются (нет `initData` → прежнее поведение).

Ограничение: из Mini App нельзя надёжно вызвать произвольный `intent://` через `openLink`; нужен тап и/или внешний браузер.

## Язык интерфейса

UI выбирается по `navigator.languages` (без внешних библиотек): `ru`, `uk`, `en`, `de`, `fr`, `es`, `it`, `pl`, `lt`, `lv`, `et`, `pt`, `nl`, `tr`. Для `be` используется русский. Иначе — английский.

Принудительно: `?lang=de` (и т.п.).

https://us-kavalchuk.github.io/BydNavi/?lat=53.86&lon=27.45&name=Test&lang=en

## Параметры

| Параметр | Описание |
| --- | --- |
| `lat`, `lon` | Широта и долгота назначения, оба обязательны |
| `name` | Подпись места на странице |
| `lat_from`, `lon_from` | Точка старта (нужны оба). Без них — от текущего места |
| `app` | Какой навигатор открыть сразу. По умолчанию `yandex` |
| `lang` | Язык UI: `en`, `ru`, `de`, … |

Старт и финиш:

https://us-kavalchuk.github.io/BydNavi/?lat=53.86&lon=27.45&lat_from=53.90&lon_from=27.56&name=Test

Сразу открыть другой навигатор (жёлтая кнопка всё равно ведёт в Яндекс Навигатор):

https://us-kavalchuk.github.io/BydNavi/?lat=53.86&lon=27.45&name=Test&app=waze

Значения `app`: `yandex` (по умолчанию), `waze`, `google`, `2gis`, `apple`, `here`, `sygic`, `organic`, `osmand`, `geo`.

## Форматы ссылок

Координаты назначения: `lat=53.86`, `lon=27.45`.

| Приложение | URL |
| --- | --- |
| Яндекс Навигатор | `yandexnavi://build_route_on_map?lat_to=53.86&lon_to=27.45` |
| Waze (https / fallback) | `https://waze.com/ul?ll=53.86,27.45&navigate=yes` |
| Waze (Android intent) | `intent://ul?ll=53.86,27.45&navigate=yes#Intent;scheme=waze;package=com.waze;S.browser_fallback_url=…;end` |
| Waze (iOS) | `waze://?ll=53.86,27.45&navigate=yes` |
| Google Maps (Android intent) | `intent://q=53.86,27.45&mode=d#Intent;scheme=google.navigation;package=com.google.android.apps.maps;S.browser_fallback_url=…;end` |
| 2GIS (https) | `https://2gis.ru/routeSearch/rsType/car/to/27.45,53.86` |
| 2GIS (`dgis://`) | `dgis://2gis.ru/routeSearch/rsType/car/to/27.45,53.86` |
| Google Maps | `https://www.google.com/maps/dir/?api=1&destination=53.86,27.45&travelmode=driving` |
| Apple Maps | `https://maps.apple.com/?daddr=53.86,27.45&dirflg=d` |
| HERE WeGo | `https://share.here.com/r/mylocation/53.86,27.45?m=d` |
| Sygic | `https://go.sygic.com/directions?to=53.86,27.45` |
| Organic Maps | `om://v2/nav?origin=currentLocation&destination=53.86,27.45&mode=drive` |
| OsmAnd | `https://osmand.net/map/?pin=53.86,27.45#14/53.86/27.45` |
| Android geo: | `geo:53.86,27.45` |

У 2GIS в пути координаты идут как **долгота,широта**. С `lat_from`/`lon_from` добавляется `from/...`, где поддерживается.

Источники: [Яндекс Навигатор](https://yandex.ru/dev/navigator/doc/ru/concepts/navigator-url-params), [2ГИС](https://help.2gis.ru/question/razrabotchikam-zapusk-ideystviya-vmobilnom-prilozhenii-cherez-deeplink), [Google Maps URLs](https://developers.google.com/maps/documentation/urls/get-started), [Waze Deep Links](https://developers.google.com/waze/deeplinks), [Apple Map Links](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html), [HERE share route](https://docs.here.com/wego-pro/docs/share-route-dl), [Sygic](https://www.sygic.com/developers/professional-navigation-sdk/ios/custom-url), [Organic Maps API](https://github.com/organicmaps/api-ios), [OsmAnd map URLs](https://osmand.net/docs/user/plan-route/web/).

Яндекс для Навигатора 2.40+ просит подпись ссылки ключом доступа (`client` и `signature`). Ключа в этом репозитории нет, поэтому ссылка собирается без подписи.
