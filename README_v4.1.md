# SXNT UI Library v4.1 — що нового

## Бінд і колір прямо в кнопках
```lua
tab:AddToggle({ name="Box", flag="Box", keybind=true, color={ flag="BoxColor", default="#FF4141", callback=function(c) end } })
card = tab:AddExpandableCard({ key="esp", flag="ESP", keybind={ key="esp" }, color={ default="#82A5FF" } })
card:AddMiniSwitch({ name="X", flag="X", keybind=true, color=Color3.new(1,0,0) })
```
`keybind` = `true` або таблиця `{ default="Q+One", mode="toggle"|"hold", modes, list=false, name, key }`.
`color` = `Color3` / `"#hex"` або `{ default, flag, callback(color, alpha), alpha, rainbow, presets, live }`.
Повертається `api.Bind` і `api.Color`. ПКМ по кнопці бінду або чип режиму перемикає Toggle / Hold.

## Інше
- `Window:AddBind{ name, keys="Q+One", mode, onPress, onToggle }`, `SubTab:AddKeybind`, `SubTab:AddColorPicker`
- HUD: `Window:AddWatermark / SetWatermark / AddKeybindList / SetKeybindList / SetKeybindDocked / ResetHud`
- `Window:AddSettingsTab{}` — готова вкладка (вигляд, HUD, клавіші, конфіги); `Library:SetAccent(color)` / `ClearAccent()`
- `Tab:AddSubTab(name, key)` — назва пілюлі перекладається разом з інтерфейсом
- `Library.Unloaded` — `true` після `Library:Unload()` (для зупинки своїх циклів)
- Конфіги зберігають кольори (`#RRGGBB`) та позиції HUD
