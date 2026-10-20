# WinIsland

A Dynamic Island-style widget for Windows 11. It lives at the top edge of your screen, expands when you point at the
edge, and cycles through four modules: **Clock**, **Resources** (CPU, memory, GPU, disk, network, battery),
**Music** (whatever is playing in Spotify, a browser, ...) and **Todo**. C# / .NET 8 / WPF.

```
Hover the top edge  → the island opens         Mouse wheel      → next / previous module (wraps around)
Pointer on island   → ← / → switch module      Ctrl+Alt+I       → show / hide (configurable)
Tray icon           → left click toggles, right click = menu     Starting it again → opens Settings
```

## What it does

| Module | Shows | Notes |
|---|---|---|
| Clock | time, date, time zone | 12/24 h or follow Windows; seconds optional |
| Resources | CPU, memory, GPU, disk, network, battery | one-second samples while visible; sparklines for CPU and GPU |
| Music | title, artist, album art, progress, previous / play-pause / next | any app that reports to Windows' media controls; opens briefly when a new track starts |
| Todo | persistent task list | SQLite; click the box to type, Enter adds, Esc leaves |

Also: settings window (applies live), multi-monitor placement, four screen positions plus left/right edge, light/dark/system
theme, accent colour, opacity, size, auto-rotation (5/10/30/60 s), start with Windows, global hotkey, stays away from
fullscreen apps and games, and a plugin system (`docs/PLUGINS.md`).

## Where things are stored

| What | Where |
|---|---|
| Settings | `%APPDATA%\WinIsland\settings.json` (the Settings window edits it live; edit the file only while the app is closed) |
| Tasks | `%LOCALAPPDATA%\WinIsland\todo.db` |
| Logs | `%LOCALAPPDATA%\WinIsland\logs\` (7 days) |
| Plugins | `%APPDATA%\WinIsland\plugins\<Name>\<Name>.dll` |

Every setting is documented in `docs/SETTINGS.md`.

## Layout

```
WinIsland.Core       contracts, models, pure logic (no Windows dependencies)         net8.0
WinIsland.Services   settings, cursor tracking, navigation, rotation, sampler, media, plugins   net8.0-windows
WinIsland.Data       SQLite todo repository                                         net8.0
WinIsland.Modules    Clock / Resources / Music / Todo modules and their views        net8.0-windows (WPF)
WinIsland.App        the island window, settings window, tray, hotkeys, startup      net8.0-windows (WPF)
WinIsland.Tests      xUnit tests
WinIsland.SamplePlugin   a complete example plugin
installer/           MSIX packaging          docs/   architecture, plugins, settings, verification
tools/               dev checks (tools/xamlvalues.py: XAML attribute values that only fail at run time)
```
