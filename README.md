# PDev-Nametags

Clean overhead player nametags for FiveM — white outlined name, a red health-bar underline, live talking indicators (whisper / normal / shout), and synced death & laststand status. Standalone, with automatic **pma-voice** and **ESX / QBCore** support.

---

## ✨ Features

- **Toggleable nametags** — each player runs `/Togglenametags` to turn theirs on/off.
- **Your own tag included** — shows above your head too (third person / mirrors).
- **Health bar** — `/TogglenametagsH` adds a health (and armor) bar under the name. Solid **red** by default, fully colour-configurable, and can be disabled server-wide.
- **Talking indicators** — `Whisper... / Normal... / Shout...` above a talking player (via pma-voice). Falls back to `Talking...` without pma-voice.
- **Synced death & laststand** — `** Dead **` / `** Yaralı **` above downed players, synced to everyone.
- **Proximity limited** — you only see other tags within a configurable radius (default **3.0** units).
- **Hex colours everywhere** — every colour is a `#RRGGBB` / `#RRGGBBAA` hex code in the config.
- **Remembers toggles** per PC between sessions.

---

## 📦 Installation

1. Copy the **`PDev-Nametags`** folder into your server's `resources` directory.
2. Add this to your `server.cfg`:
   ```cfg
   ensure PDev-Nametags
   ```
3. *(Optional, recommended)* Ensure **pma-voice** **before** this resource so whisper/shout modes are detected:
   ```cfg
   ensure pma-voice
   ensure PDev-Nametags
   ```
4. Restart the server (or `refresh` + `ensure PDev-Nametags` from console).

---

## 🎮 Commands

| Command | Description |
| --- | --- |
| `/Togglenametags` | Toggle overhead nametags on/off for yourself. |
| `/TogglenametagsH` | Toggle the health bar (only if `Config.HealthBar.enabled = true`). |

Command names are configurable in `config.lua` → `Config.Commands`.

---

## ⚙️ Configuration

All settings live in **`config.lua`**. Colours are **hex strings** — `"#RRGGBB"` or `"#RRGGBBAA"` (the `#` is optional).

### General
| Setting | Default | Purpose |
| --- | --- | --- |
| `Config.MaxDistance` | `3.0` | Radius other tags are visible within. |
| `Config.ShowOwnNametag` | `true` | Show your own tag above your head. |
| `Config.ShowServerId` | `true` | Append `(id)` → `Sweyn Forkbeard (3)`. |
| `Config.NameScale` | `0.50` | Base text size. |
| `Config.NameColor` | `#FFFFFF` | Name text colour. |
| `Config.PersistToggles` | `true` | Remember toggle choices per PC. |

### Health bar (`Config.HealthBar`)
| Setting | Default | Purpose |
| --- | --- | --- |
| `enabled` | `true` | **Master switch.** `false` disables the feature and the `/TogglenametagsH` command. |
| `defaultOn` | `true` | Health bar shown by default when nametags are on. |
| `useHealthColor` | `false` | `false` = always `color`; `true` = green→yellow→red gradient. |
| `color` | `#FF2A2A` | Solid bar colour (red). Change this hex to recolour the bar. |
| `width` / `height` | `0.058` / `0.010` | Bar size. |
| `backColor` | `#000000CC` | Track behind the bar. |
| `showArmor` | `true` | Thin armor segment above the bar. |
| `armorColor` | `#5A96EB` | Armor colour. |

**Recolour the bar** → just edit one line:
```lua
Config.HealthBar.color = '#00E5FF'   -- any hex you like
```

**Gradient by health instead of solid red:**
```lua
Config.HealthBar.useHealthColor = true
```

### Talking indicator (`Config.Talking`)
`Config.Talking.modes.whisper / normal / shout` each have a `label` and hex `color`. `voiceResource` is `'pma-voice'` by default.

### Synced status (`Config.Status`)
`dead` and `laststand` each have a `label` and hex `color`.

- `detection` = `auto` (universal — any framework + any ambulance), `standalone` (native death only), or `external` (you push status yourself).
- `ambulance` = `auto` to **auto-detect the running ambulance job**, or force one by name (e.g. `'wasabi_ambulance'`).
- `deadStates` / `laststandStates` = statebag keys scanned to decide status. Add your own if your medical script uses a different key.

**Auto-detected ambulance scripts:** `wasabi_ambulance`, `qbx_medical`, `qb-ambulancejob`, `esx_ambulancejob`, `ars_ambulance`, `aj-medical`, `codem-ambulancejob`, and more. On start it prints which source it found in the server console. If yours isn't detected, either add its statebag key to `deadStates` / `laststandStates`, or use `external` mode (below).

---

## 🩸 Custom death / laststand (external mode)

Not on ESX/QBCore? Set `Config.Status.detection = 'external'` and push status from your own resource:

```lua
exports['PDev-Nametags']:SetStatus('dead')       -- mark local player dead
exports['PDev-Nametags']:SetStatus('laststand')  -- mark local player downed / injured
exports['PDev-Nametags']:SetStatus(nil)          -- clear (alive)
```

---

## 🔌 Exports

| Export | Returns / Effect |
| --- | --- |
| `AreNametagsOn()` | boolean |
| `IsHealthBarOn()` | boolean |
| `SetStatus(status)` | set local synced status (`'dead'`/`'laststand'`/`nil`) |
| `GetStatus(serverId)` | read a player's synced status |

---

## 🧩 Compatibility

| System | Support |
| --- | --- |
| Standalone | ✅ nametags, health, talking (death via native only) |
| pma-voice | ✅ whisper / normal / shout |
| ESX / QBCore / Qbox | ✅ auto death & laststand |
| wasabi_ambulance | ✅ auto-detected (export + statebag) |
| Other ambulance jobs | ✅ auto-detected via statebags (ars, aj-medical, codem, …) |
| Any custom medical | ✅ via `external` mode or a custom statebag key |

---

*Branded as **PDev-Nametags**.*
