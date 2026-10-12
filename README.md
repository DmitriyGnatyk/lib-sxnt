# SXNT UI Library v4.0 · Premium

Бібліотека меню для Roblox (Luau): вікно з вкладками, під-вкладками, toggle, slider, dropdown, кнопками, розгортними картками, сповіщеннями, темами та мультимовністю. Усе анімоване (hover, ripple, ковзні індикатори, плавні відкриття та закриття).

**Нове у v4.0:** повноцінний **ColorPicker** (HSV + прозорість + HEX + rainbow), **система біндів із комбінаціями** (Q+1, режими toggle/hold/press), **HUD** (водяний знак + список біндів, перетягується й стикується), **готовa вкладка налаштувань** та **кастомний акцент** поверх будь-якої теми.

> Версія 4.0 повністю сумісна за API з 1.x, 2.x, 3.0–3.3. Старі скрипти працюють без змін.

---

### Що нового у 4.0

**ColorPicker.** Компонент `Sub:AddColorPicker(opts)` з повноцінною панеллю: SV-квадрат, повзунок відтінку, повзунок прозорості (опційно), поле HEX, режим rainbow, пресети кольорів. Працює в режимі «live» (колір оновлюється під час перетягування) або відкладеного callback. Можна вбудувати прямо в рядок toggle через `color = { ... }` — тоді праворуч з'явиться кружечок кольору, клік по якому відкриває поповер.

**Система біндів.** `Sub:AddKeybind(opts)` тепер підтримує:
- **Комбінації** — до 3 клавіш разом (`"Q+One"`, `"LeftShift+F"`).
- **Режими** — `toggle` (перемикач), `hold` (утримання), `press` (одноразова дія). Перемикаються ПКМ або чипом режиму.
- **Кнопки миші** — `"MB2"` (права) і `"MB3"` (середня).
- **Глобальний реєстр** — усі бінди одного вікна працюють через спільний менеджер з одним набором слухачів.
- **Вбудовані аксесуари** — клавіша/комбінація прямо в рядку toggle через `keybind = { ... }`.
- **Кнопка меню** — теж бінд, тож її можна змінити на комбінацію.

**HUD.** Водяний знак і список біндів:
- Обидва перетягуються мишею/дотиком, позиції зберігаються у конфіг.
- Список може **стикуватися** під водяним знаком і рухатися разом із ним (окремий toggle).
- Відображення лише активних біндів, режим «показати всі / тільки увімкнені».
- Автоматичне підсвічування активних рядків та коротка «спалах» для режиму press.
- Оновлення позицій, розмірів і видимості батчиться через `_hudRefresh`, щоб не бити по FPS.
- FPS/ping-лічильник працює лише поки знак видимий.

**Акцент.** `Library:SetAccent(c1, c2)` накладає кастомний акцент поверх будь-якої теми (другий колір обчислюється автоматично зі зсувом відтінку, якщо не вказано). Переживає зміну теми, доки його не скинуто (`Library:ClearAccent`). Зберігається у конфіг як `#RRGGBB` через прапор `sxnt_accent`.

**Вкладка налаштувань.** `Window:AddSettingsTab(opts)` створює готову вкладку з чотирма розділами:
- **Appearance** — тема, шрифт, акцент, мова, blur, приховування іконки, гучність звуків.
- **HUD** — водяний знак, список біндів, пристикування, лише активні, дозвіл перетягування, скидання позицій.
- **Keys** — клавіша меню, головний перемикач біндів, підказка щодо комбінацій.
- **Config** — збереження/завантаження/видалення/автозавантаження + експорт/імпорт JSON.

Кожен розділ можна вимкнути через `show = { appearance = false, hud = false, keys = false, config = false }`.

**Конфіги** тепер зберігають:
- позиції HUD (`__hud = { wm = {x, y}, kb = {x, y} }`),
- кольори як `#RRGGBB` (замість втрачених `Color3`),
- тему, шрифт, мову (як і в 3.3),
- режими біндів (через прапори `<flag>_mode`).

Керується через `Library.ConfigSaveHud` (за замовчуванням `true`).

**Оптимізація.** Декоративні анімації (частинки, обертові градієнти, пульс іконки) та охоронець миші тепер працюють **лише поки меню відкрите**, а не постійно. Це помітно економить кадри у фоні.

**Меню-клавіша як бінд.** Замість хардкод-обробника в `RenderStepped` клавіша меню тепер звичайний бінд у спільному реєстрі, тож її можна задати комбінацією і бачити в списку (з опцією `list = false`, якщо не треба показувати).

---

### Що нового у 3.3

**Шрифти.** `Library:SetFont(name)` змінює шрифт усього інтерфейсу на льоту (близько 35 шрифтів, список у `Library.FontOrder`). Готовий селектор: `Sub:AddFontPicker()`, у ньому кожен пункт показаний власним шрифтом. Свої шрифти додаються через `Library:AddFont`.

**Теми.** 28 тем: 24 темні та 4 світлі (Snow, Paper, Lavender, MintLight). Усі підсвічування та натискання адаптуються до світлої теми. Готовий селектор: `Sub:AddThemePicker()`.

**Конфіги** зберігають і відновлюють тему, шрифт та мову (`Library.ConfigSaveAppearance`, за замовчуванням `true`).

**Аудит і виправлення** — деталі в історії нижче.

---

### Що нового у 3.2

- **Dropdown: нижні пункти тепер можна вибрати.** Про натискання всередині повідомляють самі елементи списку, а не лише координати (на мобільних вони можуть бути зсунуті на `GuiInset`). Те саме для скасування запису в Keybind.
- Розгорнутий список сам прокручує сторінку, якщо виходить за видиму область.
- Видимих пунктів у dropdown стало 6, смуга прокрутки товща.

---

### Що нового у 3.1

**Анімації**
- **Пружинний рушій.** Рухи (відкриття вікна, ковзні індикатори, перемикач, повзунок, висота карток і dropdown, сповіщення, іконка) рахуються пружинами. Пружина зберігає швидкість при зміні цілі, тому анімацію можна переривати будь-коли без ривків.
- Колірні й прозорі переходи — Sine InOut, рухи — Quint Out.
- Перемикач під час натискання витягується, повзунок плавно рухається за курсором.
- Вкладки м'яко зсуваються й масштабуються при перемиканні.

**Виправлені баги**
- Keybind скасовується при закритті меню або кліку поза кнопкою.
- Textbox і пошук знімають фокус при закритті меню.
- Пошук глобальний: якщо на активній сторінці немає збігів, меню саме перемикається на вкладку, де вони є.
- InfoRow: колір, заданий через `Set`/`SetColor`, не скидається при зміні теми.
- Конфіги: імена з кирилицею більше не вважаються порожніми.
- Resize: вікно не можна розтягнути більше за екран.

---

### Що нового у 3.0

- **Виправлено «чорне» вікно.** `UIGradient` на `CanvasGroup` множить усіх дітей на свої кольори. Тепер градієнти лежать на окремих `Frame`.
- **Преміум-вигляд:** рухома градієнтна рамка, ambient-світіння, ripple на вкладках, поетапна поява.
- **Глобальний пошук** по компонентах.
- **Нові компоненти:** `AddKeybind`, `AddTextbox`, `AddInfoRow`, `AddProfileCard`, `AddStats`, `AddDivider`.
- **Конфіги:** збереження, завантаження, список, видалення, автозавантаження.
- **Водяний знак** (FPS / ping) і опційний **blur** фону.
- `RegisterTranslations` тепер **додає** ключі до вбудованих, а не замінює таблицю.

---

## Зміст

1. [Підключення](#підключення)
2. [Швидкий старт](#швидкий-старт)
3. [Library (глобальний API)](#library-глобальний-api)
4. [Window](#window)
5. [Tab і SubTab](#tab-і-subtab)
6. [Компоненти](#компоненти)
7. [ColorPicker](#colorpicker)
8. [Keybinds і комбінації](#keybinds-і-комбінації)
9. [HUD: водяний знак + список біндів](#hud-водяний-знак--список-біндів)
10. [Вкладка налаштувань](#вкладка-налаштувань)
11. [ExpandableCard і Mini-компоненти](#expandablecard-і-mini-компоненти)
12. [Прапори (Flags)](#прапори-flags)
13. [Теми та акцент](#теми-та-акцент)
14. [Шрифти](#шрифти)
15. [Мови та переклади](#мови-та-переклади)
16. [Сповіщення](#сповіщення)
17. [Welcome Splash](#welcome-splash)
18. [Керування й поведінка](#керування-й-поведінка)
19. [Повний приклад](#повний-приклад)
20. [Вирішення проблем](#вирішення-проблем)

---

## Підключення

```lua
local Library = loadstring(game:HttpGet(
    "https://raw.githubusercontent.com/DmitriyGnatyk/lib-sxnt/refs/heads/main/lib"
))()
```

GUI кріпиться до `CoreGui`, якщо це дозволено, інакше до `PlayerGui`. Повторне створення вікна автоматично прибирає старе (разом з його підключеннями).

---

## Швидкий старт

```lua
local Library = loadstring(game:HttpGet("<URL>"))()

local Window = Library:CreateWindow({
    title = "SXNT",
    theme = "Ice",
    language = "UA",
    watermark = { showFps = true, showPing = true },
})

local Tab = Window:AddTab({ icon = "◉", name = "Main" })
local Sub = Tab:AddSubTab("General")

Sub:AddSection("Combat")
Sub:AddToggle({
    name = "Enabled", flag = "enabled", default = false,
    keybind = { default = "Q+One", mode = "toggle" },   -- вбудований бінд Q+1
    color = { default = "#FF3B5C" },                    -- вбудований колір
    callback = function(v) print("Enabled:", v) end,
})
Sub:AddSlider({ name = "Speed", flag = "speed", min = 0, max = 100, default = 20, suffix = "%" })
Sub:AddButton({ name = "Hello", primary = true, callback = function()
    Window:Notify({ title = "Hi", content = "Button clicked", type = "success" })
end })

Window:AddSettingsTab()   -- готова вкладка налаштувань
Window:Open()
```

За замовчуванням меню відкривається клавішею **F4** (на ПК) або натисканням на круглу іконку. Іконку можна перетягувати.

---

## Library (глобальний API)

| Метод / поле | Опис |
|---|---|
| `Library:CreateWindow(opts)` | Створює вікно, повертає `Window`. |
| `Library:GetFlag(name, default)` | Поточне значення прапора або `default`. |
| `Library:SetFlag(name, value)` | Задає значення прапора (оновлює UI і викликає `callback`). |
| `Library:RegisterFlag(name, get, set, default)` | Реєструє власний прапор. |
| `Library:OnFlagChanged(name, fn)` | Підписка на зміни прапора, `fn(value)`. |
| `Library:GetTheme()` | Таблиця активної теми (живе дзеркало). |
| `Library:GetThemeName()` | Назва активної теми. |
| `Library:SetTheme(name, opts)` | Змінює тему для всіх вікон. `opts.onApplied(name)` викликається одразу. |
| `Library:AddTheme(name, tbl)` | Додає власну тему. |
| **`Library:SetAccent(c1, c2?)`** | **Накладає кастомний акцент поверх будь-якої теми.** `c2` обчислюється автоматично, якщо не вказано. |
| **`Library:ClearAccent()`** | **Знімає кастомний акцент.** |
| **`Library:GetAccentOverride()`** | **Поточний акцент або `nil`.** |
| `Library:SetFont(name)` / `GetFont()` | Змінити / отримати шрифт інтерфейсу. |
| `Library:AddFont(name, regular, bold)` | Додати власний шрифт (`Enum.Font`). |
| `Library.Fonts` / `Library.FontOrder` | Таблиця шрифтів і масив назв. |
| `Library:GetLanguage()` / `SetLanguage(code)` | Поточна мова / зміна мови (`ENG`, `UA`, `TR`, `RUS`). |
| `Library:RegisterTranslations(tbl)` | Додає ключі до вбудованих. |
| `Library:T(key)` | Повертає переклад ключа. |
| `Library:ShowWelcome(opts)` | Показує вітальну заставку. |
| `Library:Unload()` | Знищує всі вікна бібліотеки. |
| `Library.Version` | `"4.0"` |
| `Library.Copy(text)` | Копіює в буфер обміну, повертає `true/false`. |
| `Library:ExportConfig()` / `ImportConfig(tbl)` | Таблиця прапорів (bool / number / string / Color3) і її застосування. |
| `Library:SaveConfig(name)` / `LoadConfig(name)` | Збереження/завантаження JSON у `Library.ConfigFolder`. |
| `Library:ListConfigs()` / `DeleteConfig(name)` | Список збережених конфігів / видалення. |
| `Library:SetAutoload(name)` / `GetAutoload()` / `AutoLoad()` | Конфіг, що застосовується при запуску. |
| `Library.ConfigFolder` | Папка конфігів (`"SXNT"`). |
| `Library.ConfigIgnore` | `[flag] = true`, щоб не зберігати прапор у конфіг. |
| **`Library.ConfigSaveHud`** | **Чи зберігати позиції HUD (за замовчуванням `true`).** |
| `Library.ConfigSaveAppearance` | Чи зберігати тему / шрифт / мову (за замовчуванням `true`). |
| `Library.BindRespectGameProcessed` | `false` (за замовчуванням): бінди працюють і на клавішах, які вже обробила гра (Shift Lock, ProximityPrompt); блокуються лише під час набору тексту. `true` — стара поведінка. |
| `Library.Themes` / `ThemeOrder` | Таблиця всіх тем і масив назв. |
| `Library.Defaults` | `{ avatar = "rbxassetid://..." }` |
| **`Library._hud`** | **Спільний стан HUD (позиції, видимість).** |

### Опції `CreateWindow`

| Опція | Тип | За замовчуванням | Опис |
|---|---|---|---|
| `title` | string | `"SXNT"` | Заголовок у шапці. |
| `width`, `height` | number | `500`, `360` | Початковий розмір (400–720 × 280–520). |
| `theme` | string | `"Ice"` | Початкова тема. |
| `font` | string | `"Gotham"` | Початковий шрифт. |
| `language` | string | `"ENG"` | Початкова мова. |
| `translations` | table | вбудовані | Власні переклади. |
| `avatar` | string | вбудований | `rbxassetid://...` для іконки меню. |
| `soundVolume` | number | `1.2` | Гучність клік-звуків. |
| `menuKey` | `Enum.KeyCode` | `F4` | Клавіша відкриття (керується через бінд `__menu`). |
| `displayOrder` | number | `100` | `DisplayOrder` ScreenGui. |
| `version` | string / `false` | `"V4.0"` | Бейдж біля заголовка. |
| `blur` | boolean | `false` | Розмиття гри під меню, поки воно відкрите. |
| `lockMouseOnClose` | boolean | `false` | Якщо `true`, при закритті миша завжди в `LockCenter`. |
| **`watermark`** | boolean / table | — | **Автоматично додає водяний знак (див. HUD).** |
| **`keybindList`** | boolean | `true` | **Чи показувати список біндів автоматично при першому toggle з біндом.** |
| `onOpen` / `onClose` | function | — | Колбеки відкриття / закриття. |
| `onThemeRequested` | function(name) | — | Після зміни теми. |

---

## Window

Об'єкт, який повертає `CreateWindow`.

| Метод | Опис |
|---|---|
| `Window:AddTab(opts)` | Додає вкладку. Повертає `Tab`. |
| `Window:SelectTab(index)` | Програмно перемикає вкладку (від 1). |
| `Window:Notify(opts)` | Показує сповіщення. |
| `Window:Open()` / `Close()` / `Toggle()` | Керування видимістю. |
| `Window:IsOpen()` | `true`, якщо вікно відкрите. |
| `Window:Destroy()` | Знищує вікно, відключає події, відновлює мишу. |
| `Window:SetTheme(name)` / `SetFont(name)` / `SetLanguage(code)` | Те саме, що `Library:*`. |
| `Window:GetThemeName()` / `GetFont()` / `GetLanguage()` | Геттери. |
| **`Window:AddWatermark(opts)`** | **Водяний знак (FPS/ping, перетягується).** |
| **`Window:SetWatermark(bool)` / `IsWatermarkVisible()`** | **Показати/сховати знак.** |
| **`Window:AddKeybindList(opts)`** | **Список біндів (перетягується, стикується).** |
| **`Window:SetKeybindList(bool)` / `IsKeybindListVisible()`** | **Керування списком.** |
| **`Window:SetKeybindOnlyActive(bool)`** | **Показувати лише активні бінди.** |
| **`Window:SetKeybindDocked(bool)`** | **Пристикувати список під водяним знаком.** |
| **`Window:SetHudDraggable(bool)` / `IsHudDraggable()`** | **Дозвіл перетягування HUD.** |
| **`Window:ResetHud()`** | **Скинути позиції до типових і пристикувати список.** |
| **`Window:AddBind(opts)`** | **Публічний API бінда (поза компонентом).** |
| **`Window:SetBindsEnabled(bool)` / `AreBindsEnabled()`** | **Головний перемикач біндів.** |
| **`Window:AddSettingsTab(opts)`** | **Готова вкладка налаштувань.** |
| `Window:SetBlur(bool)` / `GetBlur()` | Blur фону при відкритому меню. |
| `Window.ScreenGui`, `Window.Main` | Кореневий `ScreenGui` і головний `CanvasGroup`. |

### `AddTab`

```lua
local Tab = Window:AddTab({ icon = "◉", name = "Main", key = "tab_main" })
local Tab2 = Window:AddTab("Settings")   -- скорочена форма
```

| Поле | Опис |
|---|---|
| `name` | Текст вкладки. |
| `icon` | Символ перед текстом (за замовчуванням `◉`). |
| `key` | Ключ перекладу. |

Перша створена вкладка відкривається автоматично.

---

## Tab і SubTab

```lua
local Tab = Window:AddTab("Main")
local General = Tab:AddSubTab("General")
local Extra   = Tab:AddSubTab("Extra")
```

`Tab:AddSubTab(name)` створює «пілюлю» у верхньому ряду вкладки. Перша під-вкладка активна за замовчуванням.

---

## Компоненти

### Загальне для компонентів

| Поле | Опис |
|---|---|
| `name` | Підпис. |
| `key` | Ключ перекладу. |
| `flag` | Ім'я прапора. |
| `callback` | Функція зворотного виклику. |
| **`keybind`** | **Вбудувати бінд у рядок (для toggle): `true` або `{ default, mode, flag, name, list, callback, modes, showMode }`.** |
| **`color`** | **Вбудувати чіп кольору (для toggle): `Color3` / `"#hex"` або `{ default, flag, callback, alpha, rainbow, presets, live }`.** |
| `desc` | Опис під назвою (у toggle). |

### `AddSection(text, key)`

Заголовок секції з тонкою лінією. Повертає `TextLabel`.

### `AddLabel(text)`

Текст із переносом. Повертає `TextLabel`.

### `AddButton(opts)`

| Поле | Опис |
|---|---|
| `name` | Текст кнопки. |
| `primary` | `true` дає кнопку з градієнтом акцентних кольорів. |
| `callback()` | Викликається при кліку. |

Повертає `TextButton`.

### `AddToggle(opts)`

Повертає `host, api`. `api:Get()`, `api:Set(v)`. Якщо вказано `keybind` / `color` — ще й `api.Bind` / `api.Color`.

```lua
local _, toggle = Sub:AddToggle({
    name = "Aimbot", flag = "aimbot",
    keybind = { default = "Q+One", mode = "toggle" },
    color   = { default = "#FF3B5C", rainbow = true },
    callback = function(v) print("Aimbot:", v) end,
})
toggle:Set(true)
toggle.Bind:Set("F+G")            -- змінити комбінацію
toggle.Color:Set("#00FF88")       -- змінити колір
```

### `AddSlider(opts)`

| Поле | Опис |
|---|---|
| `min`, `max` | Межі (`0`, `100`). |
| `step` | Крок (`1`). Кількість знаків після коми береться з кроку. |
| `default` | Початкове значення. |
| `suffix` | Текст після числа. |
| `callback(value)` | Викликається лише коли значення змінилось. |

Повертає `hit, api`.

### `AddDropdown(opts)`

| Поле | Опис |
|---|---|
| `options` / `items` | Масив значень. |
| `default` | Початкове значення. |
| `callback(value)` | Викликається при виборі. |
| `getValue()` / `setValue(value)` | Кастомні геттер/сетер початкового значення. |
| `swatch(option)` | Повертає `Color3, Color3` для двох кольорових «крапель». |
| `optionFont(option)` | Повертає `Enum.Font` для пункту. |

Повертає `api`:

```lua
local dd = Sub:AddDropdown({ name = "Mode", flag = "mode", options = {"A", "B", "C"}, default = "A" })
dd:Get() dd:Set("B") dd:SetOptions({"X", "Y"}, "X") dd:Refresh() dd:SetSilent("C")
```

---

## ColorPicker

### `Sub:AddColorPicker(opts)`

Стандартна картка: назва + кружечок поточного кольору. Клік розгортає панель.

| Поле | Опис |
|---|---|
| `name` / `key` / `flag` | Як в інших компонентів. |
| `default` | `Color3` / `"#RRGGBB"` / `{ r, g, b }` (0-255). |
| `alpha` | `true` — додає повзунок прозорості. Значення зберігається у прапор `<flag>_alpha`. |
| `rainbow` | `true` — додає перемикач rainbow. Прапор `<flag>_rainbow`. |
| `presets` | Масив кольорів. `false` — вимкнути рядок пресетів. |
| `live` | `true` (за замовчуванням) — callback кличеться під час перетягування. `false` — лише після відпускання. |
| `expanded` | Розгорнути одразу. |
| `callback(color, alpha)` | Викликається при зміні. |

Повертає `api`:

```lua
local cp = Sub:AddColorPicker({
    name = "ESP Color", flag = "esp_color", default = "#FF3B5C",
    alpha = true, rainbow = true, live = true,
    callback = function(c, a) print(c, a) end,
})
cp:Get()             -- Color3
cp:GetAlpha()        -- 0..1
cp:GetHex()          -- "#FF3B5C"
cp:GetHSV()          -- h, s, v
cp:Set("#00FF88")    -- задати колір
cp:SetAlpha(0.5)
cp:SetRainbow(true)
cp:IsRainbow()       -- true/false
cp:Expand() cp:Collapse() cp:IsExpanded()
```

### Вбудований чіп у toggle

```lua
Sub:AddToggle({
    name = "Highlight",
    color = { default = "#4BE37B", rainbow = true, alpha = true },
})
```

Праворуч у рядку з'явиться маленький кружечок кольору. Клік — відкриває поповер із повноцінною панеллю. Клік по колірному чіпу під час відкритого дропдауна автоматично закриває його.

### Готове API

Об'єкт `ColorUI` (внутрішній):

```lua
local m = ColorUI.chip(win, container, order, opts, H)   -- кружечок кольору
m.model, m.api, m.button
```

---

## Keybinds і комбінації

### `Sub:AddKeybind(opts)`

| Поле | Опис |
|---|---|
| `name` / `key` / `flag` | Як в інших. |
| `default` | Клавіша: `"Q+One"`, `"LeftShift+F"`, `Enum.KeyCode.E`, `{ "Q", Enum.KeyCode.One }`. |
| `mode` | `"toggle"` / `"hold"` / `"press"`. За замовчуванням `toggle`, якщо є `target` або `onToggle`; інакше `press`. |
| `modes` | Масив доступних режимів для перемикання ПКМ. |
| `showMode` | Показувати чип режиму. За замовчуванням `true`, якщо є `target` або `onToggle`. |
| `list` | Чи показувати у списку HUD (за замовчуванням `true`). |
| `listName` | Ім'я в списку HUD (окремо від `name`). |
| `target` | Що перемикати: рядок (ім'я прапора), функція (приймає `state`), таблиця з `:Set(state)`. |
| `onPress()` | Спрацьовує при натисканні (для будь-якого режиму). |
| `onToggle(state)` | Для режимів `toggle`/`hold`. |
| `callback(keyCode, combo, keys)` | При зміні клавіші. |
| `required` | `true` — не можна стерти (наприклад, клавіша меню). |

Повертає `host, api`:

```lua
local _, kb = Sub:AddKeybind({
    name = "Aim key", flag = "aim_key",
    default = "Q+One", mode = "toggle", modes = { "toggle", "hold", "press" },
    target = "aimbot_enabled",   -- перемикає прапор
    onPress = function() print("pressed") end,
})
kb:Get()            -- Enum.KeyCode першої клавіші (сумісність із v3)
kb:GetCombo()       -- "Q+One"
kb:Set("F+G")       -- змінити комбінацію
kb:GetMode()        -- "toggle"
kb:SetMode("hold")
kb:IsActive()       -- true/false
kb:SetActive(true)
kb:SetEnabled(false)
kb:SetListed(false)
kb:SetName("Інша назва")
kb:Remove()         -- видалити бінд
```

### Вбудований бінд у toggle

```lua
Sub:AddToggle({
    name = "Fly",
    keybind = {
        default = "Q+One",
        mode = "toggle",
        modes = { "toggle", "hold" },
        flag = "fly_bind",              -- за замовчуванням "<flag>_bind"
        list = true,
        callback = function(keyCode, combo, keys) end,
    },
})
```

Бінд **автоматично синхронізується** зі станом перемикача: натиснув комбінацію — toggle увімкнувся; клікнув toggle — бінд теж оновиться.

### `Window:AddBind(opts)` (публічний API)

Для біндів без UI (наприклад, глобальна гаряча клавіша скрипта):

```lua
local bind = Window:AddBind({
    id = "my_bind", name = "My Action",
    keys = "F8", mode = "press", list = false,
    onPress = function() print("F8!") end,
})
bind:Set("F9")
```

### Як поводяться комбінації

- **До 3 клавіш** в одній комбінації.
- При конфлікті перемагає **найдовша** комбінація: `Q+1` перекриває одиночну `Q`.
- Якщо існує довша комбінація з тією ж клавішею, коротша чекає **0.14 с** — щоб дати час натиснути другу клавішу.
- Якщо довшої немає — спрацьовує одразу, без затримки.
- **Esc** скасовує запис. **Backspace/Delete** очищає. Захоплення до 3 клавіш одночасно.
- Маленький **×** праворуч від клавіші одним кліком прибирає бінд (його немає лише в обов'язкових, наприклад клавіша меню).
- **ПКМ** або клік по чипу режиму перемикає `toggle → hold → press`.

---

## HUD: водяний знак + список біндів

Спільний стан живе в `Library._hud`, тож переживає перестворення вікна і потрапляє в конфіг.

### `Window:AddWatermark(opts)`

| Поле | Опис |
|---|---|
| `text` | Напис-лого (за замовчуванням `"SXNT"`). |
| `showFps` / `showPing` | Показати FPS / ping (за замовчуванням `true`). |
| `visible` | Початкова видимість. |
| `position` | `Vector2` або `UDim2` — початкова позиція, якщо ще не збережена. |
| `draggable` | Дозволити перетягування (за замовчуванням `true`). |

Водяний знак має динамічну ширину (AutomaticSize), кольоровий градієнт рамки, FPS/ping-лічильник (працює лише коли знак видимий).

### `Window:AddKeybindList(opts)`

| Поле | Опис |
|---|---|
| `visible` | Початкова видимість. |
| `title` | Заголовок (за замовчуванням переклад `hud_keybinds`). |
| `onlyActive` | Показувати лише активні бінди (за замовчуванням `false`). |
| `docked` | Пристикувати під водяним знаком (за замовчуванням `true`). |

Кожен рядок: кольорова точка + назва + комбінація. Точка загоряється акцентом, коли бінд активний. Для режиму `press` — короткий «спалах» 0.35 с.

Пристикований список **рухається разом із водяним знаком**. Якщо перетягнути список — рухається вся група. Відстикований — окремо.

### Керування HUD

```lua
Window:SetWatermark(true)          -- показати знак
Window:SetKeybindList(true)        -- показати список
Window:SetKeybindOnlyActive(true)  -- лише активні
Window:SetKeybindDocked(false)     -- відстикувати
Window:SetHudDraggable(false)      -- заборонити перетягування
Window:ResetHud()                  -- скинути позиції
```

Усі перемикачі доступні як прапори для налаштувань і конфігів:

| Прапор | Призначення |
|---|---|
| `sxnt_wm_visible` | Видимість водяного знака |
| `sxnt_kb_visible` | Видимість списку біндів |
| `sxnt_kb_active_only` | Лише активні бінди |
| `sxnt_kb_docked` | Пристикування списку |
| `sxnt_hud_draggable` | Дозвіл перетягування |

---

## Вкладка налаштувань

### `Window:AddSettingsTab(opts)`

Створює готову вкладку з налаштуваннями. Повертає `Tab`.

```lua
Window:AddSettingsTab({
    icon = "⚙", key = "tab_settings",
    show = {
        appearance = true,   -- тема / шрифт / акцент / мова / blur / звук
        hud = true,          -- HUD-перемикачі
        keys = true,         -- клавіша меню + головний перемикач біндів
        config = true,       -- збереження / завантаження / JSON
    },
})
```

**Appearance** — селектор теми, селектор шрифту, ColorPicker акценту, кнопка «Скинути акцент», мова, blur, приховування іконки (не на мобільному), гучність звуків.

**HUD** — 5 toggle (водяний знак, список, лише активні, пристикування, дозвіл перетягування) + кнопка скидання позицій.

**Keys** — Keybind клавіші меню (обов'язковий, не можна стерти), головний перемикач «Бінди увімкнено», текстова підказка.

**Config** — ім'я конфігу, випадаючий список збережених, кнопки Save / Load / Autoload / Delete, кнопка «Копіювати конфиг як JSON», Textbox для імпорту JSON.

---

## ExpandableCard і Mini-компоненти

```lua
local Card = Sub:AddExpandableCard({
    name = "Aimbot", flag = "aimbot", default = false,
    callback = function(v) print("Aimbot", v) end,
})

Card:AddMiniSwitch({ name = "Team check", flag = "aim_team", default = true })
Card:AddMiniSlider({ name = "FOV", flag = "aim_fov", min = 10, max = 360, default = 90, suffix = "°" })
Card:AddMiniButton({ name = "Reset", callback = function() print("reset") end })

local Group = Card:AddMiniExpandable({ name = "Advanced", flag = "aim_adv" })
Group.AddMiniSwitch({ name = "Prediction", flag = "aim_pred" })
Group:AddMiniSlider({ name = "Smooth", flag = "aim_smooth", min = 1, max = 20, default = 5 })
```

Міні-компоненти всередині картки теж підтримують `keybind` і `color`.

| Метод | Опції |
|---|---|
| `Card:AddMiniSwitch(opts)` | як у `AddToggle` (+ `keybind`, `color`) |
| `Card:AddMiniSlider(opts)` | як у `AddSlider` |
| `Card:AddMiniButton(opts)` | `name`, `callback`, `active` |
| `Card:AddMiniExpandable(opts)` | `name`, `flag`, `default`, `callback` |
| `Card:Refresh()` | примусово перерахувати висоту |

---

## Прапори (Flags)

```lua
Sub:AddToggle({ name = "ESP", flag = "esp" })

print(Library:GetFlag("esp", false))
Library:SetFlag("esp", true)
Library:OnFlagChanged("esp", function(v) print("ESP:", v) end)
```

### Власні прапори

```lua
local myValue = 5
Library:RegisterFlag("custom",
    function() return myValue end,
    function(v) myValue = v end,
    5)
```

### Кольори в прапорах

`Color3` не можна серіалізувати в JSON — тому `ExportConfig` автоматично конвертує їх у `#RRGGBB`. `ImportConfig` розбирає назад. Прапори `<flag>_alpha`, `<flag>_rainbow`, `<flag>_mode` зберігаються окремо.

---

## Теми та акцент

Вбудовано **28 тем** (`Library.ThemeOrder`):

- **Темні:** Ice, Purple, Crimson, Toxic, Midnight, Sunset, Aurora, Rose, Ocean, Sky, Mint, Forest, Cyber, Amethyst, Dracula, Nord, Monokai, Gold, Lava, Blood, Sakura, NeonPink, Coffee, Graphite.
- **Світлі:** Snow, Paper, Lavender, MintLight.

```lua
Library:SetTheme("Purple")
Window:SetTheme("Sunset")
print(Library:GetThemeName())
```

### Кастомний акцент

```lua
Library:SetAccent(Color3.fromRGB(0, 200, 120))            -- c2 обчислиться автоматично
Library:SetAccent("#0096FF", "#7C5BFF")                   -- задати обидва
print(Library:GetAccentOverride())                        -- Color3 або nil
Library:ClearAccent()                                     -- зняти
```

Акцент **накладається поверх теми**: кольори `Theme.accent`, `Theme.accent2` і `Theme.onAccent` перераховуються. Прапор `sxnt_accent` (`#RRGGBB` або `""`) зберігає його в конфіг.

### Готові селектори

```lua
Sub:AddThemePicker()   -- прапор sxnt_theme
Sub:AddFontPicker()    -- прапор sxnt_font
```

### Власна тема

```lua
Library:AddTheme("MyTheme", {
    bg = Color3.fromRGB(10, 18, 28),
    card = Color3.fromRGB(18, 30, 44),
    accent = Color3.fromRGB(0, 200, 220),
    accent2 = Color3.fromRGB(80, 120, 255),
    text = Color3.fromRGB(230, 240, 250),
    muted = Color3.fromRGB(100, 125, 150),
    border = Color3.fromRGB(35, 55, 75),
})
```

---

## Шрифти

```lua
Library:SetFont("Ubuntu")
print(Library:GetFont())
Library:CreateWindow({ font = "Nunito" })
Sub:AddFontPicker()   -- прапор sxnt_font
```

**Доступні шрифти** (`Library.FontOrder`):
Gotham, Gotham Semibold, Gotham Black, Builder Sans, Source Sans, Source Sans Light, Arial, Ubuntu, Roboto, Roboto Condensed, Roboto Mono, Nunito, Montserrat, Josefin Sans, Oswald, Titillium Web, Inconsolata, Jura, Michroma, Sarpanch, Merriweather, Garamond, Bodoni, Code, Sci-Fi, Arcade, Highway, Cartoon, Fredoka One, Patrick Hand, Indie Flower, Kalam, Special Elite, Permanent Marker, Amatic SC, Bangers, Fantasy, Antique.

```lua
Library:AddFont("My Mix", Enum.Font.Code, Enum.Font.Arcade)
```

> Декоративні шрифти можуть не мати кирилиці. Для UA/RUS надійні: Gotham, Source Sans, Arial, Ubuntu, Roboto, Nunito, Merriweather.

---

## Мови та переклади

Вбудовані мови: `ENG`, `UA`, `TR`, `RUS`. Вбудовані ключі:
`copy`, `copied`, `lang`, `keybind`, `bindkey`, `hideicon`, `theme`, `sound`, `volume`, `cant_hide_mobile`, `cleared`, `welcome_new`, `welcome_back`, `loading`, `entering`, `search`, `none`, `no_clipboard`.

**v4.0 додає ключі:**
`mode_toggle`, `mode_hold`, `mode_press`, `rainbow`, `hud_keybinds`, `tab_settings`, `sub_appearance`, `sub_hud`, `sub_keys`, `sub_config`, `sec_theme`, `sec_interface`, `sec_sound`, `sec_hud`, `sec_binds`, `sec_configs`, `sec_transfer`, `accent_color`, `accent_reset`, `blur`, `hud_watermark`, `hud_keybinds_show`, `hud_only_active`, `hud_docked`, `hud_draggable`, `hud_reset`, `binds_master`, `keys_hint`, `cfg_name`, `cfg_pick`, `cfg_save`, `cfg_load`, `cfg_delete`, `cfg_autoload`, `cfg_export`, `cfg_import`, `cfg_nofs`, `cfg_saved`, `cfg_loaded`, `cfg_deleted`, `cfg_err`, `cfg_empty`, `cfg_copied`, `cfg_autoset`, `cfg_none`, `cfg_imported`.

```lua
Library:SetLanguage("UA")
print(Library:T("theme"))
```

```lua
Library:RegisterTranslations({
    ENG = { tab_main = "Main", aim = "Aimbot" },
    UA  = { tab_main = "Головна", aim = "Аімбот" },
})
```

---

## Сповіщення

```lua
Window:Notify({
    title = "Saved",
    content = "Config saved",
    type = "success",     -- info | success | warning | error
    duration = 4,         -- 0 = не закривати
})
Window:Notify("Просто текст")
```

Стекаються у правому верхньому куті (до 5). Працюють, навіть коли меню закрите. Повертають `card, dismiss`.

---

## Welcome Splash

```lua
task.spawn(function()
    Library:ShowWelcome({
        language = "UA",
        isReturning = true,
        brand = "SXNT",
        onComplete = function() Window:Open() end,
    })
end)
```

**Блокуючий виклик** — запускай у `task.spawn`.

---

## Керування й поведінка

| Дія | Результат |
|---|---|
| **F4** (`menuKey`) | відкрити або закрити меню (ПК) |
| Клік по іконці | відкрити меню |
| Перетягування іконки | переміщення (клік < 6 px) |
| Перетягування шапки | переміщення вікна |
| Ручка `◢` | зміна розміру |
| Клік поза dropdown | закриває його |
| Заголовок `ExpandableCard` | вмикає/вимикає + розгортає |
| **ПКМ по кнопці бінда** | циклічно перемикає режим |
| **Esc під час запису** | скасувати |
| **Backspace / Delete** | очистити |
| **Перетягування HUD** | рухає знак / список |

Додатково:

- **Миша.** Поки меню відкрите, курсор вільний. Охоронець миші підключається **лише на час відкриття** (економія кадрів).
- **Мобільні.** Автомасштаб, іконку сховати не можна, гаряча клавіша вимкнена.
- **Оптимізація.** Частинки, обертання, пульс іконки паузяться, коли меню закрите.
- **Звук.** Кожна `TextButton` грає клік. Вимкнути: `soundVolume = 0`.

---

## Повний приклад

```lua
local Library = loadstring(game:HttpGet("<URL>"))()

local Window = Library:CreateWindow({
    title = "SXNT", theme = "Purple", language = "UA",
    menuKey = Enum.KeyCode.F4,
    watermark = { showFps = true, showPing = true },
    onOpen  = function() print("opened") end,
    onClose = function() print("closed") end,
})

-- Головна
local Main = Window:AddTab({ icon = "◉", key = "tab_main" })
local General = Main:AddSubTab("General")

General:AddSection("Movement")
General:AddToggle({
    name = "Fly", flag = "fly",
    keybind = { default = "Q+One", mode = "toggle" },
    color = { default = "#4BE37B", rainbow = true },
    callback = function(v) print("Fly", v) end,
})
General:AddSlider({ name = "Speed", flag = "speed", min = 16, max = 120, default = 16, suffix = " sps" })

-- Кольори й бінди
General:AddSection("Visuals")
General:AddColorPicker({
    name = "ESP Color", flag = "esp_color", default = "#FF3B5C",
    alpha = true, rainbow = true, live = true,
    callback = function(c, a) print("color:", c, a) end,
})
General:AddKeybind({
    name = "Panic Key", flag = "panic_key", default = "F8", mode = "press",
    onPress = function() print("PANIC") end,
})

-- HUD
General:AddButton({ name = "Test notification", primary = true, callback = function()
    Window:Notify({ title = "Test", content = "Everything works", type = "success" })
end })

-- Готова вкладка налаштувань
Window:AddSettingsTab()

Library:OnFlagChanged("speed", function(v)
    local hum = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.WalkSpeed = v end
end)

task.spawn(function()
    Library:ShowWelcome({ language = "UA", onComplete = function() Window:Open() end })
end)
```

---

## Вирішення проблем

**Усе вікно темне.**
Ніколи не вішай `UIGradient` на `CanvasGroup`, `Window.Main` чи вкладки. Клади градієнт на окремий `Frame` усередині.

**Меню не з'являється.**
Перевір, що викликаєш `Window:Open()` або натискаєш `menuKey`. Іконка з'являється автоматично, якщо вікно закрите.

**Бінд не спрацьовує.**
- Перевір, що клавіша меню не конфліктує з комбінацією (наприклад, `F4` + `F4` + `Q`).
- Перевір `Library._hud.draggable` — на ПК перетягування HUD не заважає біндам.
- Подивись у список біндів — там видно, чи зареєстровано бінд.

**Комбінація Q+1 не працює, а Q працює.**
Перевір, що обидва ключі правильно названі: `"Q"` і `"One"`. У бібліотеці використовуються **назви `Enum.KeyCode`**: `One`, `Two`, ..., `Zero`, `LeftShift`, `RightControl`, `Backquote` тощо.

**Список біндів не з'являється.**
- Перевір `Window._kbAutoShow` (за замовчуванням `true`).
- Перевір прапор `sxnt_kb_visible`.
- Список порожній, якщо жоден бінд не має `list = true`.

**Позиція HUD не зберігається.**
`Library.ConfigSaveHud` має бути `true` (за замовчуванням). Позиції пишуться у `__hud` експорту конфігу.

**Колір не зберігається в конфіг.**
`ExportConfig` конвертує `Color3` → `#RRGGBB`. Переконайся, що прапор не в `Library.ConfigIgnore`.

**Переклад не змінюється.**
Перевір, що елементи створені з `key`, ключ існує, і для потрібної мови є значення (інакше береться `ENG`).

**Після закриття меню миша не блокується.**
Передай `lockMouseOnClose = true`.

**Помилки типу `attempt to index nil` у моєму колбеку.**
Усі колбеки через `pcall`, тож помилки не ламають UI. У консолі з'явиться `[SXNT] помилка в колбеку: ...` (не частіше раза на 5 с).

**FPS просів після v4.0.**
Перевір, скільки біндів у `list = true`. Якщо їх багато і вони активні постійно — вимкни `onlyActive` або зменш список.
