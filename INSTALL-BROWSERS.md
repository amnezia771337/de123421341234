# Браузеры Playwright 1.59.1 для Windows (x64)

Инструкция для **другого компьютера с Windows**. На этой машине ничего искать не обязательно — достаточно скачать архивы и положить их в нужное место на Windows.

## Версии, которые ждёт Playwright 1.59.1

| Браузер   | Папка в кэше     | Архив (win64)        |
|-----------|------------------|----------------------|
| Chromium  | `chromium-1217`  | `chrome-win64.zip` (у вас уже есть) |
| **Chrome Headless Shell** | `chromium-headless-shell-1217` | `chrome-headless-shell-win64.zip` |
| Firefox   | `firefox-1511`   | `firefox-win64.zip`  |
| WebKit    | `webkit-2272`    | `webkit-win64.zip`   |

Ревизии взяты из официального `browsers.json` для тега **v1.59.1**.

**Chrome Headless Shell** — это не «тот же zip, что обычный Chrome»: отдельный урезанный бинарник для headless-режима. Ревизия **1217** совпадает с Chromium, но каталог в кэше **другой** (`chromium-headless-shell-1217`). Если его нет, `npx playwright install` или тесты, которым нужен этот канал, докачают его отдельно.

## Куда «перетащить» (стандартный кэш Windows)

1. Откройте проводник и вставьте в адресную строку:
   ```text
   %LOCALAPPDATA%\ms-playwright
   ```
   Обычно это:
   ```text
   C:\Users\<ВашПользователь>\AppData\Local\ms-playwright
   ```

2. В этой папке должны лежать **каталоги** с именами **точно** как в таблице (`chromium-1217`, `chromium-headless-shell-1217` при необходимости, `firefox-1511`, `webkit-2272`), а не сами zip-файлы.

3. **Chromium:** распакуйте ваш `chrome-win64.zip` так, чтобы содержимое оказалось внутри:
   ```text
   %LOCALAPPDATA%\ms-playwright\chromium-1217\
   ```
   (внутри — структура как после официальной установки Playwright: папка `chrome-win64` и т.п., как в архиве; главное — корень распаковки = `chromium-1217`.)

3b. **Chrome Headless Shell (по желанию):** скачайте `chrome-headless-shell-win64.zip` (ссылка ниже) и распакуйте **содержимое** в:
   ```text
   %LOCALAPPDATA%\ms-playwright\chromium-headless-shell-1217\
   ```
   Внутри должен быть каталог `chrome-headless-shell-win64` с `chrome-headless-shell.exe`.

4. **Firefox:** скачайте архив и распакуйте **содержимое** `firefox-win64.zip` в:
   ```text
   %LOCALAPPDATA%\ms-playwright\firefox-1511\
   ```
   Внутри `firefox-1511` должен появиться каталог `firefox` с исполняемым файлом Firefox.

5. **WebKit:** скачайте архив и распакуйте **содержимое** `webkit-win64.zip` в:
   ```text
   %LOCALAPPDATA%\ms-playwright\webkit-2272\
   ```

Если после распаковки лишний верхний уровень (например, одна папка внутри zip) — перенесите **внутреннее** содержимое в `firefox-1511` / `webkit-2272` / `chromium-1217` / `chromium-headless-shell-1217`, чтобы пути совпали с тем, что создаёт `playwright install`.

## Откуда скачать Firefox и WebKit (официальные сборки)

Прямые ссылки на CDN Playwright (редирект на Microsoft CDN — это нормально):

- Firefox:  
  `https://playwright.azureedge.net/builds/firefox/1511/firefox-win64.zip`
- WebKit:  
  `https://playwright.azureedge.net/builds/webkit/2272/webkit-win64.zip`

Chromium в 1.59.1 берётся как **Chrome for Testing** (не из `builds/chromium/…`), версия **147.0.7727.15** — из `browsers.json`. Ссылка для ручной загрузки win64:

`https://playwright.azureedge.net/builds/cft/147.0.7727.15/win64/chrome-win64.zip`

**Тот же Chrome for Testing (147.0.7727.15), отдельный архив для headless shell:**

`https://playwright.azureedge.net/builds/cft/147.0.7727.15/win64/chrome-headless-shell-win64.zip`

(Редирект на `playwright.download.prss.microsoft.com` — нормально.) Имя обычного Chrome может совпадать с вашим уже скачанным `chrome-win64.zip`.

Сохраните zip в любую папку на Windows, затем распакуйте в каталоги выше.

## Вариант: свой каталог (не в AppData)

1. Создайте, например, `D:\playwright-browsers` и положите туда те же папки `chromium-1217`, при необходимости `chromium-headless-shell-1217`, `firefox-1511`, `webkit-2272`.

2. Перед запуском тестов задайте переменную окружения (PowerShell для текущей сессии):
   ```powershell
   $env:PLAYWRIGHT_BROWSERS_PATH = "D:\playwright-browsers"
   ```
   Или добавьте **системную** / **пользовательскую** переменную `PLAYWRIGHT_BROWSERS_PATH` с этим путём в «Параметры Windows → Система → О системе → Дополнительные параметры → Переменные среды».

Playwright будет искать браузеры только в указанной папке.

## Самый простой способ на Windows (без ручной распаковки)

На целевом ПК с Node.js в папке проекта:

```powershell
npx playwright@1.59.1 install firefox webkit
```

Чтобы подтянуть и headless shell вместе с Chromium:

```powershell
npx playwright@1.59.1 install chromium chromium-headless-shell
```

При необходимости полный набор:

```powershell
npx playwright@1.59.1 install
```

Это само скачает нужные ревизии в `%LOCALAPPDATA%\ms-playwright`. Ручные zip нужны, если **нет интернета** на том ПК или вы **переносите** кэш с флешки.

## Краткий чеклист переноса с флешки

1. На машине с интернетом скачать `firefox-win64.zip` и `webkit-win64.zip` (ссылки выше).
2. Распаковать в правильные имена папок или скопировать уже готовый каталог `ms-playwright` целиком с другой машины, где выполняли `playwright install`.
3. На целевом ПК: либо вставить в `%LOCALAPPDATA%\ms-playwright`, либо указать `PLAYWRIGHT_BROWSERS_PATH` на корень, где лежат `firefox-1511` и `webkit-2272`.

Версия Playwright в проекте должна совпадать с **1.59.1**, иначе понадобятся другие номера папок (другие ревизии из другого `browsers.json`).
