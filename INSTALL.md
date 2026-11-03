# Установка (Windows, шаг за шагом)

Инструкция написана так, чтобы её можно было выполнить, даже если ты первый раз видишь
терминал. Делай по порядку, после каждого блока проверяй результат.

---

## 0. Что должно быть установлено

| Программа | Проверка | Если нет |
|---|---|---|
| OpenCode | `opencode --version` | https://opencode.ai |
| Node.js (LTS) | `node --version` | https://nodejs.org |
| Roblox Studio | ярлык на рабочем столе | https://www.roblox.com/download/studio |

Подписка **OpenCode GO** должна быть оплачена, иначе модели из пресета не заработают.

---

## 1. Подключить MCP-серверы

Без них агент не увидит Roblox Studio. Полная инструкция с разбором ошибок — в репозитории
`MCP_SETUP_PROMPT.md`, здесь только суть.

1. Открой Roblox Studio, открой любой place.
2. В Studio: **Assistant → ⋯ → Manage MCP Servers → включи «Enable Studio as MCP server»**.
3. Открой файл `%USERPROFILE%\.config\opencode\opencode.jsonc` и добавь блок `mcp`
   из `config/opencode.jsonc` этого репозитория (не заменяй свои настройки целиком —
   добавь только ключи `Roblox_Studio`, `blender`, `chromium`).
4. Перезапусти OpenCode.

Проверка: в OpenCode выполни `/mcps` — у `Roblox_Studio` должно быть **Connected**.

> Если показывает Error — закрой OpenCode полностью и открой заново. Не держи открытыми
> одновременно desktop-версию и терминальную: каждая поднимает свой мост к Studio,
> а порт может занять только один.

---

## 2. Поставить плагин oh-my-opencode-slim

В терминале:

```powershell
npx oh-my-opencode-slim@latest install
```

Установщик сам добавит `"plugin": ["oh-my-opencode-slim"]` в конфиг OpenCode и создаст
файл `%USERPROFILE%\.config\opencode\oh-my-opencode-slim.json`.

---

## 3. Скопировать пресет roblox

Скопируй файлы из этого репозитория:

| Откуда | Куда |
|---|---|
| `config/oh-my-opencode-slim.jsonc` | `%USERPROFILE%\.config\opencode\oh-my-opencode-slim.jsonc` |
| `config/prompts/roblox/*.md` | `%USERPROFILE%\.config\opencode\oh-my-opencode-slim\roblox\` |

Папку `roblox` создай, если её нет.

Затем в `oh-my-opencode-slim.jsonc` проверь строку:

```jsonc
"preset": "roblox",
```

Важно: файл `.jsonc` имеет приоритет над `.json`. Если после установки плагина остался
старый `oh-my-opencode-slim.json` с другим пресетом — либо удали его, либо переименуй
в `.jsonc` и приведи к нашему виду.

---

## 4. Перезапустить и проверить

1. Закрой OpenCode полностью (desktop — из трея, не просто окно).
2. Открой заново.
3. Проверь, что видишь агентов: в интерфейсе должна быть команда `/preset` и пресет `roblox`.

Быстрый тест — попроси:

> Покажи дерево открытого места в Roblox Studio, но не меняй ничего.

Если агент вызвал инструменты Roblox и вернул список объектов — всё работает.

---

## 5. Если что-то не так

| Проблема | Что делать |
|---|---|
| `Roblox_Studio` в статусе Error | перезапусти OpenCode; проверь, что place открыт; `Get-Process StudioMCP` — должен быть один процесс |
| Модель не найдена / 404 | модель убрали из Go-плана — открой `oh-my-opencode-slim.jsonc` и замени на другую из https://opencode.ai/v2/docs/console/go/ |
| Плагин не загрузился | `npx oh-my-opencode-slim@latest install` ещё раз, потом перезапуск |
| Агенты не появились | проверь, что в `opencode.jsonc` есть `"plugin": ["oh-my-opencode-slim"]` |
| Лимиты кончились | подожди 5 часов (лимит обновляется) или включи Extra Usage в Console |

---

## 6. Куда смотреть дальше

- `README.md` — как ставить задачи и как учиться.
- Репозиторий плагина: https://github.com/alvinunreal/oh-my-opencode-slim
- Документация Roblox: https://create.roblox.com/docs
