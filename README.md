# SXNT UI Library v3.0 · Premium

Бібліотека меню для Roblox (Luau): вікно з вкладками, під-вкладками, toggle, slider, dropdown, кнопками, розгортними картками, сповіщеннями, темами та мультимовністю. Усе анімоване (hover, ripple, ковзні індикатори, плавні відкриття та закриття).

> Версія 3.0 повністю сумісна за API з 1.x і 2.x. Старі скрипти працюють без змін.

### Що нового у 3.0

- **Виправлено «чорне» вікно.** `UIGradient` на `CanvasGroup` множить усіх дітей на свої кольори, тому все меню ставало майже чорним. Тепер градієнти лежать на окремих `Frame` (вікно, вітальна заставка, primary-кнопки).
- **Преміум-вигляд:** рухома градієнтна рамка, ambient-світіння зверху, панель сайдбару, бейдж версії, ripple на вкладках, поетапна поява вкладок.
- **Глобальний пошук** по компонентах (поле у шапці).
- **Нові компоненти:** `AddKeybind`, `AddTextbox`, `AddInfoRow`, `AddProfileCard`, `AddStats`, `AddDivider`, опція `desc` у toggle.
- **Конфіги:** збереження, завантаження, список, видалення, автозавантаження.
- **Водяний знак** (FPS / ping) і опційний **blur** фону.
- **Нові теми:** `Aurora`, `Rose`. Читабельніший `muted`-колір у `Ice` і `Purple`.
- `RegisterTranslations` тепер **додає** ключі до вбудованих, а не замінює таблицю. Додано ключі `search`, `none`, `no_clipboard`.

---

## Зміст

1. [Підключення](#підключення)
2. [Швидкий старт](#швидкий-старт)
3. [Library (глобальний API)](#library-глобальний-api)
4. [Window](#window)
5. [Tab і SubTab](#tab-і-subtab)
6. [Компоненти](#компоненти)
7. [ExpandableCard і Mini-компоненти](#expandablecard-і-mini-компоненти)
8. [Прапори (Flags)](#прапори-flags)
9. [Теми](#теми)
10. [Мови та переклади](#мови-та-переклади)
11. [Сповіщення](#сповіщення)
12. [Welcome Splash](#welcome-splash)
13. [Керування й поведінка](#керування-й-поведінка)
14. [Повний приклад](#повний-приклад)
15. [Вирішення проблем](#вирішення-проблем)

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
})

local Tab = Window:AddTab({ icon = "◉", name = "Main" })
local Sub = Tab:AddSubTab("General")

Sub:AddSection("Combat")
Sub:AddToggle({
    name = "Enabled",
    flag = "enabled",
    default = false,
    callback = function(v) print("Enabled:", v) end,
})
Sub:AddSlider({ name = "Speed", flag = "speed", min = 0, max = 100, default = 20, suffix = "%" })
Sub:AddButton({ name = "Hello", primary = true, callback = function()
    Window:Notify({ title = "Hi", content = "Button clicked", type = "success" })
end })

Window:Open()
```

За замовчуванням меню відкривається клавішею **F4** (на ПК) або натисканням на круглу іконку. Іконку можна перетягувати.

---

## Library (глобальний API)

| Метод / поле | Опис |
|---|---|
| `Library:CreateWindow(opts)` | Створює вікно, повертає `Window`. |
| `Library:GetFlag(name, default)` | Поточне значення прапора або `default`, якщо такого прапора немає. |
| `Library:SetFlag(name, value)` | Задає значення прапора (оновлює UI і викликає `callback`). |
| `Library:RegisterFlag(name, get, set, default)` | Реєструє власний прапор. |
| `Library:OnFlagChanged(name, fn)` | Підписка на зміни прапора, `fn(value)`. |
| `Library:GetTheme()` | Таблиця активної теми (живе дзеркало). |
| `Library:GetThemeName()` | Назва активної теми. |
| `Library:SetTheme(name, opts)` | Змінює тему для всіх вікон. `opts.onApplied(name)` викликається одразу після застосування. Повертає `true/false`. |
| `Library:AddTheme(name, tbl)` | Додає власну тему. |
| `Library:GetLanguage()` / `Library:SetLanguage(code)` | Поточна мова / зміна мови (`ENG`, `UA`, `TR`, `RUS`). |
| `Library:RegisterTranslations(tbl)` | Підміняє таблицю перекладів (див. [Мови](#мови-та-переклади)). |
| `Library:T(key)` | Повертає переклад ключа для поточної мови. |
| `Library:ShowWelcome(opts)` | Показує вітальну заставку. |
| `Library:Unload()` | Знищує всі вікна цієї бібліотеки. |
| `Library.Version` | `"3.0"` |
| `Library.Copy(text)` | Копіює в буфер обміну (якщо executor дозволяє), повертає `true/false`. |
| `Library:ExportConfig()` / `ImportConfig(tbl)` | Таблиця прапорів (bool / number / string) і її застосування. |
| `Library:SaveConfig(name)` / `LoadConfig(name)` | Збереження й завантаження JSON у `Library.ConfigFolder`. Повертають `ok, errOrCount`. |
| `Library:ListConfigs()` / `DeleteConfig(name)` | Список збережених конфігів / видалення. |
| `Library:SetAutoload(name)` / `GetAutoload()` / `AutoLoad()` | Конфіг, що застосовується при запуску. |
| `Library.ConfigFolder` | Папка конфігів (`"SXNT"`). |
| `Library.ConfigIgnore` | `[flag] = true`, щоб не зберігати прапор у конфіг. |
| `Library.Themes` | Таблиця всіх тем. |
| `Library.ThemeOrder` | Масив назв тем (для dropdown-ів). |
| `Library.Defaults` | `{ avatar = "rbxassetid://..." }` |

### Опції `CreateWindow`

| Опція | Тип | За замовчуванням | Опис |
|---|---|---|---|
| `title` | string | `"SXNT"` | Заголовок у шапці. |
| `width`, `height` | number | `500`, `360` | Початковий розмір вікна. Користувач може змінити розмір (400–720 × 280–520). |
| `theme` | string | `"Ice"` | Початкова тема. |
| `language` | string | `"ENG"` | Початкова мова. |
| `translations` | table | вбудовані | Власні переклади (замінює вбудовані). |
| `avatar` | string | вбудований | `rbxassetid://...` для іконки меню. |
| `soundVolume` | number | `1.2` | Гучність клік-звуків (`0` вимикає). |
| `menuKey` | `Enum.KeyCode` | `F4` | Клавіша відкриття (не на мобільних). |
| `displayOrder` | number | `100` | `DisplayOrder` ScreenGui. |
| `version` | string / `false` | `"V3.0"` | Текст бейджа біля заголовка. `false` ховає бейдж. |
| `blur` | boolean | `false` | Розмиття гри під меню, поки воно відкрите. |
| `lockMouseOnClose` | boolean | `false` | Якщо `true`, при закритті меню миша завжди переходить у `LockCenter` (поведінка v1). Інакше відновлюється стан, який був до відкриття. |
| `onOpen` | function | | Викликається при відкритті. |
| `onClose` | function | | Викликається при закритті. |
| `onThemeRequested` | function(name) | | Викликається після зміни теми. Кольори вже оновлені самі, тож перебудовувати меню зазвичай не треба. |

---

## Window

Об'єкт, який повертає `CreateWindow`.

| Метод | Опис |
|---|---|
| `Window:AddTab(opts)` | Додає вкладку (див. нижче). Повертає `Tab`. |
| `Window:SelectTab(index)` | Програмно перемикає вкладку (від 1). |
| `Window:Notify(opts)` | Показує сповіщення ([деталі](#сповіщення)). |
| `Window:Open()` / `Close()` / `Toggle()` | Керування видимістю. |
| `Window:IsOpen()` | `true`, якщо вікно відкрите. |
| `Window:Destroy()` | Знищує вікно, відключає всі події, відновлює мишу. |
| `Window:SetTheme(name)` | Те саме, що `Library:SetTheme`. |
| `Window:AddWatermark(opts)` | Плашка з FPS і ping (див. [нижче](#водяний-знак-blur-і-пошук)). |
| `Window:SetWatermark(bool)` / `IsWatermarkVisible()` | Показати або сховати водяний знак. |
| `Window:SetBlur(bool)` / `GetBlur()` | Blur фону при відкритому меню. |
| `Window:SetLanguage(code)` | Те саме, що `Library:SetLanguage`. |
| `Window:GetLanguage()` / `GetThemeName()` | Геттери. |
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
| `key` | Ключ перекладу. Якщо не вказано, використовується `name`. |

Перша створена вкладка відкривається автоматично.

### Службові (приватні) члени, сумісні з v1

Використовуються в налаштуваннях меню (іконка, звук, клавіша):

```lua
Window._hideIcon(true)            -- сховати іконку (ігнорується на мобільних)
Window._SetVolume(0.8)            -- гучність клік-звуків
Window._GetVolume()
Window._menuKeySetter(Enum.KeyCode.RightShift)
Window._menuKeyGetter()
Window._icon                      -- TextButton іконки
Window._isMobile                  -- boolean
Window._textElements              -- реєстр текстів для перекладу
```

---

## Tab і SubTab

```lua
local Tab = Window:AddTab("Main")
local General = Tab:AddSubTab("General")
local Extra   = Tab:AddSubTab("Extra")
```

`Tab:AddSubTab(name)` створює «пілюлю» у верхньому ряду вкладки. Перша під-вкладка активна за замовчуванням. Елементи додаються до `SubTab`.

---

## Компоненти

Усі методи викликаються на `SubTab` через двокрапку. Усі опції необов'язкові.

### Загальне для компонентів

| Поле | Опис |
|---|---|
| `name` | Підпис. |
| `key` | Ключ перекладу. Якщо `name` не вказано, підпис береться з перекладу, а при зміні мови оновлюється сам. |
| `flag` | Ім'я прапора (див. [Прапори](#прапори-flags)). |
| `callback` | Функція зворотного виклику. |

### `AddSection(text, key)`

Заголовок секції з тонкою лінією. Повертає `TextLabel`.

```lua
Sub:AddSection("Visuals")
Sub:AddSection("Visuals", "sec_visuals")   -- з перекладом
```

### `AddLabel(text)`

Текст, що переноситься на кілька рядків. Повертає `TextLabel` (можна міняти `.Text`).

### `AddButton(opts)`

| Поле | Опис |
|---|---|
| `name` | Текст кнопки. |
| `primary` | `true` дає кнопку з градієнтом акцентних кольорів. |
| `callback()` | Викликається при кліку. |

Має ripple-ефект. Повертає `TextButton`.

### `AddToggle(opts)`

| Поле | Опис |
|---|---|
| `default` | Початковий стан (`false`). |
| `callback(value)` | Викликається при зміні. |

Клікабельна вся картка. Повертає `host, api`, де `api:Get()` і `api:Set(v)`.

```lua
local _, toggle = Sub:AddToggle({ name = "ESP", flag = "esp" })
toggle:Set(true)
print(toggle:Get())
```

### `AddSlider(opts)`

| Поле | Опис |
|---|---|
| `min`, `max` | Межі (`0`, `100`). Якщо `max <= min`, `max` стає `min + 1`. |
| `step` | Крок (`1`). Кількість знаків після коми береться з кроку (`0.05` дає 2 знаки). |
| `default` | Початкове значення. |
| `suffix` | Текст після числа (`"%"`, `" studs"`). |
| `callback(value)` | Викликається лише коли значення реально змінилось. |

Повертає `hit, api` (`api:Get()`, `api:Set(v)`).

### `AddDropdown(opts)`

| Поле | Опис |
|---|---|
| `options` / `items` | Масив значень. |
| `default` | Початкове значення (інакше перший елемент). |
| `callback(value)` | Викликається при виборі. |
| `getValue()` | Необов'язково: початкове значення береться звідси. |
| `setValue(value)` | Необов'язково: додатковий колбек при виборі користувачем. |

Повертає `api`:

```lua
local dd = Sub:AddDropdown({ name = "Mode", flag = "mode", options = {"A", "B", "C"}, default = "A" })
dd:Get()                         -- "A"
dd:Set("B")                      -- змінює значення і викликає callback
dd:SetOptions({"X", "Y"}, "X")   -- нові варіанти і поточне значення
dd:Refresh()                     -- перебудувати список
```

Одночасно відкритий лише один dropdown. Закривається кліком поза ним, зміною вкладки та закриттям вікна. Максимум 5 пунктів видно одразу, далі список прокручується.

---

### Нові компоненти (v3.0)

Усі методи викликаються на `SubTab` і беруть участь у глобальному пошуку.

#### `AddKeybind(opts)`

Кнопка, яка чекає натискання клавіші. `Esc` скасовує, `Backspace` очищає.

```lua
local _, kb = Sub:AddKeybind({
    name = "Aim key", flag = "aim_key", default = Enum.KeyCode.E,
    callback = function(keyCode) print("bound:", keyCode) end,   -- nil, якщо очищено
    onPress  = function() print("pressed") end,                  -- коли клавішу натиснули поза режимом призначення
})
kb:Get()            -- Enum.KeyCode або nil
kb:Set("G")         -- рядок або Enum.KeyCode
```

У прапорі зберігається **назва клавіші** (`"E"`, `"None"`), тому конфіги працюють.

#### `AddTextbox(opts)`

| Поле | Опис |
|---|---|
| `name` / `key` / `flag` | Як у інших компонентів. |
| `default`, `placeholder` | Початковий текст і підказка. |
| `numeric` | `true` залишає лише цифри, `.` і `-`. |
| `callback(text, enterPressed)` | Викликається при Enter або втраті фокусу. |

Повертає `host, api` (`api:Get()`, `api:Set(text)`).

#### `AddInfoRow(opts)`

Рядок «назва ... значення». Клік копіює значення й показує сповіщення.

```lua
local _, row = Sub:AddInfoRow({
    name = "Key", value = "sxnt-ab…xyz",
    copyValue = fullKey,             -- що копіювати (рядок або функція); за замовчуванням value
    color = Color3.fromRGB(80, 220, 140),
    dot = Color3.fromRGB(80, 220, 140),   -- індикатор-«пульс» ліворуч від значення
    copy = false,                    -- вимкнути копіювання
})
row:Set("новий текст", newColor)   row:SetColor(c)   row:SetDot(c or nil)
```

#### `AddProfileCard(opts)`

Картка профілю з аватаром (обертове градієнтне кільце), ім'ям, `@ніком`, ID (клік копіює) і бейджем. Усі поля необов'язкові, за замовчуванням береться `LocalPlayer`.

```lua
local _, card = Sub:AddProfileCard({
    name = "Dmytro", username = "@sxnt_gn", userId = 123,
    badge = "PREMIUM", badgeColor = Library:GetTheme().green,
})
card:SetBadge("EXPIRED", Library:GetTheme().red)
```

#### `AddStats(list)`

Ряд плиток «велике число + підпис». Повертає масив `api` (`api:Set(value)` з анімацією, `api:SetColor(c)`).

```lua
local stats = Sub:AddStats({
    { name = "Онлайн", value = "2" },
    { name = "Ping", value = "—" },
})
stats[1]:Set("3")
```

#### `AddDivider()`

Тонка лінія з плавним затуханням по краях.

#### Опис у toggle

```lua
Sub:AddToggle({ name = "Fullbright", desc = "Прибирає темряву", flag = "fb" })
```

## Конфіги

Потрібен executor з `writefile` / `readfile` / `isfile` (для списку ще й `listfiles`, для видалення `delfile`). Зберігаються всі прапори з типом `boolean`, `number`, `string`.

```lua
Library.ConfigIgnore["cfg_name"] = true     -- службові прапори не зберігати

local ok, err = Library:SaveConfig("legit")
local ok2, applied = Library:LoadConfig("legit")   -- applied = скільки параметрів застосовано
print(Library:ListConfigs())                   -- { "legit", ... }
Library:DeleteConfig("legit")

Library:SetAutoload("legit")                   -- пустий рядок вимикає
Library:AutoLoad()                             -- викликай після створення всіх елементів
```

Без файлової системи `SaveConfig` / `LoadConfig` повертають `false, "filesystem unavailable"`. `ExportConfig()` / `ImportConfig(tbl)` працюють завжди, так що JSON можна зберігати де завгодно.

## Водяний знак, blur і пошук

```lua
Window:AddWatermark({ text = "SXNT", showFps = true, showPing = true })
Window:SetWatermark(false)      -- сховати
Window:SetBlur(true)            -- розмиття, поки меню відкрите (або blur = true у CreateWindow)
```

**Пошук** — поле в шапці. Фільтрує компоненти всіх вкладок за назвою (з урахуванням поточної мови); під час пошуку секції й розділювачі ховаються. Порожній запит повертає все назад. На вузькому вікні (менше ~454 px) поле зникає.

## ExpandableCard і Mini-компоненти

Картка із заголовком-перемикачем: коли вона увімкнена, вона розгортається і показує вкладені елементи.

```lua
local Card = Sub:AddExpandableCard({
    name = "Aimbot",
    flag = "aimbot",
    default = false,                -- за замовчуванням true, якщо не вказано
    callback = function(v) print("Aimbot", v) end,
})

Card:AddMiniSwitch({ name = "Team check", flag = "aim_team", default = true })
Card:AddMiniSlider({ name = "FOV", flag = "aim_fov", min = 10, max = 360, default = 90, suffix = "°" })
Card:AddMiniButton({ name = "Reset", callback = function() print("reset") end })

local Group = Card:AddMiniExpandable({ name = "Advanced", flag = "aim_adv" })
Group.AddMiniSwitch({ name = "Prediction", flag = "aim_pred" })
Group:AddMiniSlider({ name = "Smooth", flag = "aim_smooth", min = 1, max = 20, default = 5 })
```

Висота картки рахується автоматично, додавання елементів відбувається без стрибків, вкладені групи теж анімуються.

| Метод | Опції |
|---|---|
| `Card:AddMiniSwitch(opts)` | як у `AddToggle` |
| `Card:AddMiniSlider(opts)` | як у `AddSlider` |
| `Card:AddMiniButton(opts)` | `name`, `callback`, `active` (чи виділена кнопка) |
| `Card:AddMiniExpandable(opts)` | `name`, `flag`, `default` (`false`), `callback` |
| `Card:Refresh()` | примусово перерахувати висоту |

Об'єкт, який повертає `AddMiniExpandable`, приймає виклики **і через крапку, і через двокрапку** (`Group.AddMiniSwitch({...})` та `Group:AddMiniSwitch({...})`). Групи можна вкладати одна в одну.

---

## Прапори (Flags)

Прапор зв'язує значення компонента з іменем, щоб читати й міняти його ззовні (наприклад, для збереження конфігу).

```lua
Sub:AddToggle({ name = "ESP", flag = "esp" })

print(Library:GetFlag("esp", false))   -- поточне значення
Library:SetFlag("esp", true)           -- змінює UI і викликає callback

Library:OnFlagChanged("esp", function(v)
    print("ESP змінено на", v)         -- спрацьовує і від кліку, і від SetFlag
end)
```

### Правила

- `GetFlag` завжди повертає **поточне** значення компонента.
- Зміна користувачем (клік, перетягування, вибір) оновлює прапор і викликає підписників.
- `SetFlag` застосовує значення до UI та викликає `callback` компонента. Виняток: toggle і розгортна картка викликають його лише якщо значення відрізняється від поточного.
- Повторне створення компонента з тим самим `flag` перезаписує його реєстрацію (значення скидається до `default`), але підписки `OnFlagChanged` зберігаються.
- `SetFlag` для невідомого імені створює «голий» прапор зі значенням.

### Збереження конфігу (приклад)

```lua
local HttpService = game:GetService("HttpService")
local saved = {}
for _, name in ipairs({"esp", "speed", "mode"}) do
    saved[name] = Library:GetFlag(name)
end
local json = HttpService:JSONEncode(saved)

-- завантаження
for name, value in pairs(HttpService:JSONDecode(json)) do
    Library:SetFlag(name, value)
end
```

### Власні прапори

```lua
local myValue = 5
Library:RegisterFlag("custom",
    function() return myValue end,                  -- get
    function(v) myValue = v end,                    -- set
    5)                                              -- default
```

---

## Теми

Вбудовані: `Ice`, `Purple`, `Crimson`, `Toxic`, `Midnight`, `Sunset`, `Aurora`, `Rose` (список у `Library.ThemeOrder`).

```lua
Library:SetTheme("Purple")          -- плавне перефарбування всього вікна
Window:SetTheme("Sunset")           -- те саме
print(Library:GetThemeName())
```

### Власна тема

Обов'язкові ключі: `bg`, `card`, `accent`, `accent2`, `text`, `muted`, `border`. Решту, якщо не вказати, взято з теми Ice або розраховано автоматично.

```lua
Library:AddTheme("Ocean", {
    label   = "Ocean",
    bg      = Color3.fromRGB(10, 18, 28),
    card    = Color3.fromRGB(18, 30, 44),
    accent  = Color3.fromRGB(0, 200, 220),
    accent2 = Color3.fromRGB(80, 120, 255),
    text    = Color3.fromRGB(230, 240, 250),
    muted   = Color3.fromRGB(100, 125, 150),
    border  = Color3.fromRGB(35, 55, 75),
})
Library:SetTheme("Ocean")
```

### Ключі теми

| Ключ | Призначення |
|---|---|
| `bg` | фон вікна |
| `card`, `cardHover` | фон карток і при наведенні |
| `accent`, `accent2` | основний і додатковий акцент (градієнти) |
| `text`, `muted` | основний і приглушений текст |
| `border` | рамки й лінії |
| `green`, `red` | успіх, помилка / закриття |
| `telegram` | колір посилань (зарезервований) |
| `card2`, `track`, `amber`, `onAccent` | **похідні**, рахуються автоматично. `onAccent` — колір тексту на акценті (темний для світлих акцентів, наприклад у Midnight і Toxic). Їх можна вказати вручну. |

### Dropdown вибору теми

```lua
Sub:AddDropdown({
    name = "Theme",
    options = Library.ThemeOrder,
    default = Library:GetThemeName(),
    callback = function(name) Library:SetTheme(name) end,
})
```

---

## Мови та переклади

Вбудовані мови: `ENG`, `UA`, `TR`, `RUS`. Вбудовані ключі:

`copy`, `copied`, `lang`, `keybind`, `bindkey`, `hideicon`, `theme`, `sound`, `volume`, `cant_hide_mobile`, `cleared`, `welcome_new`, `welcome_back`, `loading`, `entering`, `search`, `none`, `no_clipboard`.

```lua
Library:SetLanguage("UA")
print(Library:T("theme"))   -- "Тема"
```

### Власні переклади

> З v3.0 `RegisterTranslations` (і опція `translations` у `CreateWindow`) **додає** ключі до вбудованих і перезаписує однакові. Копіювати вбудовані ключі більше не потрібно. Якщо ключа немає для мови, береться значення з `ENG`.

```lua
Library:RegisterTranslations({
    ENG = {
        welcome_new = "Welcome", welcome_back = "Welcome back",
        loading = "Loading menu...", entering = "Entering...",
        tab_main = "Main", aim = "Aimbot",
    },
    UA = {
        welcome_new = "Вітаємо", welcome_back = "З поверненням",
        loading = "Завантаження меню...", entering = "Вхід...",
        tab_main = "Головна", aim = "Аімбот",
    },
})
```

### Як це працює в компонентах

```lua
Window:AddTab({ icon = "◉", key = "tab_main" })        -- текст береться з перекладу
Sub:AddToggle({ key = "aim", flag = "aim" })           -- name не потрібен
Sub:AddSection("Combat", "sec_combat")
```

При `SetLanguage` усі елементи з `key` оновлюються автоматично. Якщо для ключа немає перекладу, показується значення з `ENG`, а за його відсутності сам ключ.

Для власних елементів можна зареєструвати оновлення вручну:

```lua
table.insert(Window._textElements, {
    type = "custom",
    update = function() myLabel.Text = Library:T("aim") end,
})
```

---

## Сповіщення

```lua
Window:Notify({
    title    = "Saved",
    content  = "Config saved successfully",
    type     = "success",     -- info | success | warning | error
    duration = 4,             -- секунди; 0 = не закривати автоматично
})
Window:Notify("Просто текст")   -- скорочена форма
```

- Стекаються у правому верхньому куті (до 5 одночасно, старіші зникають).
- Мають іконку за типом, progress bar таймера та кнопку закриття.
- Висота підлаштовується під довжину тексту.
- Працюють навіть коли меню закрите.
- `description` приймається як синонім `content`.
- Повертає `CanvasGroup` картки.

---

## Welcome Splash

Вітальна заставка з аватаром і прогрес-баром. **Блокуючий виклик** (використовує `task.wait`), тому за потреби запускай у `task.spawn`.

```lua
task.spawn(function()
    Library:ShowWelcome({
        language    = "UA",       -- мова напису
        isReturning = true,       -- "З поверненням" / "Вітаємо"
        avatar      = "rbxassetid://123",  -- необов'язково
        brand       = "SXNT",     -- напис-лого
        onComplete  = function() Window:Open() end,
    })
end)
```

---

## Керування й поведінка

| Дія | Результат |
|---|---|
| **F4** (`menuKey`) | відкрити або закрити меню (ПК) |
| Клік по іконці | відкрити меню |
| Перетягування іконки | переміщення (клік з рухом менше ~6 px вважається кліком) |
| Перетягування шапки | переміщення вікна, плавне, не виходить за екран |
| Ручка `◢` у кутку | зміна розміру (верхній лівий кут нерухомий) |
| Клік поза dropdown | закриває його |
| Заголовок `ExpandableCard` | вмикає/вимикає опцію і розгортає/згортає картку |

Додатково:

- **Миша.** Поки меню відкрите, курсор примусово вільний і видимий. При закритті відновлюється попередній стан (`MouseBehavior`, `MouseIconEnabled`).
- **Мобільні.** Вікно автоматично масштабується під екран, іконку сховати не можна, гаряча клавіша вимкнена.
- **Адаптація.** Вікно масштабується (до 1.0), якщо екран менший за розмір вікна, і реагує на зміну розміру екрана.
- **Звук.** Кожна `TextButton` у меню грає клік. Вимкнути можна через `soundVolume = 0` або `Window._SetVolume(0)`.

---

## Повний приклад

```lua
local Library = loadstring(game:HttpGet("<URL>"))()

Library:RegisterTranslations({
    ENG = { welcome_new = "Welcome", welcome_back = "Welcome back", loading = "Loading...", entering = "Entering...",
            theme = "Theme", sound = "Click sounds", volume = "Volume", tab_main = "Main", tab_settings = "Settings" },
    UA  = { welcome_new = "Вітаємо", welcome_back = "З поверненням", loading = "Завантаження...", entering = "Вхід...",
            theme = "Тема", sound = "Клік-звуки", volume = "Гучність", tab_main = "Головна", tab_settings = "Налаштування" },
})

local Window = Library:CreateWindow({
    title = "SXNT",
    theme = "Purple",
    language = "UA",
    menuKey = Enum.KeyCode.F4,
    onOpen  = function() print("opened") end,
    onClose = function() print("closed") end,
})

-- Головна
local Main = Window:AddTab({ icon = "◉", key = "tab_main" })
local General = Main:AddSubTab("General")

General:AddSection("Movement")
General:AddToggle({ name = "Fly", flag = "fly", callback = function(v) print("Fly", v) end })
General:AddSlider({ name = "Speed", flag = "speed", min = 16, max = 120, default = 16, step = 1, suffix = " sps" })

local Card = General:AddExpandableCard({ name = "Aimbot", flag = "aim", default = false })
Card:AddMiniSwitch({ name = "Team check", flag = "aim_team", default = true })
Card:AddMiniSlider({ name = "FOV", flag = "aim_fov", min = 10, max = 360, default = 90 })
local Adv = Card:AddMiniExpandable({ name = "Advanced", flag = "aim_adv" })
Adv.AddMiniSwitch({ name = "Prediction", flag = "aim_pred" })

General:AddDropdown({ name = "Target", flag = "target", options = {"Head", "Torso", "Random"}, default = "Head" })
General:AddButton({ name = "Test notification", primary = true, callback = function()
    Window:Notify({ title = "Test", content = "Everything works", type = "success" })
end })

-- Налаштування
local Settings = Window:AddTab({ icon = "⚙", key = "tab_settings" })
local UI = Settings:AddSubTab("UI")

UI:AddDropdown({
    key = "theme", options = Library.ThemeOrder, default = Library:GetThemeName(),
    callback = function(name) Library:SetTheme(name) end,
})
UI:AddDropdown({
    name = "Language", options = {"ENG", "UA", "TR", "RUS"}, default = Library:GetLanguage(),
    callback = function(code) Library:SetLanguage(code) end,
})
UI:AddSlider({ key = "volume", min = 0, max = 2, step = 0.1, default = 1.2,
    callback = function(v) Window._SetVolume(v) end })

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

**Усе вікно темне, ледь видно текст.**
Це був баг до v3.0: `UIGradient` на `CanvasGroup` множить увесь вміст групи на свої (дуже темні) кольори. Якщо додаєш власні елементи, ніколи не вішай `UIGradient` на `CanvasGroup`, `Window.Main` чи вкладки. Клади градієнт на окремий `Frame` усередині.

**Меню не з'являється.**
Перевір, що викликаєш `Window:Open()` або натискаєш `menuKey`. Іконка меню з'являється автоматично, якщо вікно закрите. Якщо GUI не видно, переконайся, що `GetMainGui` має доступ до `CoreGui`. Інакше воно йде в `PlayerGui`, де `ResetOnSpawn` вимкнено.

**Зміна теми не зачіпає мої власні елементи.**
Власні `Instance` бібліотека не знає. Фарбуй їх вручну в `onThemeRequested` (або після `Library:SetTheme(..., { onApplied = fn })`), беручи кольори з `Library:GetTheme()`.

**Переклад не змінюється.**
Перевір, що елементи створені з `key`, ключ існує в `RegisterTranslations` для потрібної мови, а ключ є для потрібної мови (з v3.0 `RegisterTranslations` лише додає ключі, вбудовані не зникають).

**Після закриття меню миша не блокується.**
У v2.0 відновлюється стан, який був до відкриття. Якщо твоя гра очікує `LockCenter` завжди, передай `lockMouseOnClose = true`.

**`callback` не спрацював при `SetFlag`.**
Toggle і розгортна картка викликають `callback` лише якщо нове значення відрізняється від поточного. Slider і dropdown викликають завжди.

**Повзунок із дробовим кроком показує зайві цифри.**
Кількість знаків береться з `step` (`0.1` дає 1 знак, `0.05` дає 2). Значення, менші за `1e-6`, не підтримуються.

**Помилки типу `attempt to index nil` у моєму колбеку.**
Усі колбеки бібліотеки викликаються через `pcall`, тож помилки в них не ламають UI, а тихо ігноруються. Додай власний `print` або `warn` усередині колбека для налагодження.
