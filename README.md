# dark_UI

A lightweight, draggable Roblox UI library for building in-game menus with tabs, toggles, sliders, buttons, labels, and textboxes.

---

## Features

- Tabbed navigation with smooth tween transitions
- Draggable window
- Built-in elements: Button, Toggle, Slider, Textbox, Label
- Scrollable content area per tab
- Optional startup sound
- Auto-destroys duplicate instances on load

---

## Installation

Load the library via `loadstring` or by requiring the module script directly:

```lua
local Library = loadstring(game:HttpGet("YOUR_RAW_URL_HERE"))()
```

Or if placed as a ModuleScript in your game:

```lua
local Library = require(path.to.Library)
```

---

## Quick Start

```lua
local Library = require(path.to.Library)

local Window = Library:Create({
    Name = "My Menu",
    StartupSound = {
        Toggle = false,
        SoundID = nil,
        TimePosition = 0
    }
})

local Tab = Window:Tab("Main")

Tab:Button("Click Me", function()
    print("Button clicked!")
end)

Tab:Toggle("Enable Feature", function(state)
    print("Toggle is now:", state)
end)

Tab:Slider("Speed", 50, 0, 100, function(value)
    print("Slider value:", value)
end)

Tab:Textbox("Enter name", function(text)
    print("Input:", text)
end)

Tab:Label("This is a label")
```

---

## API Reference

### `Library:Create(table)`

Creates and renders the main window. Returns a `tabHandler` object.

| Field | Type | Description |
|---|---|---|
| `Name` | `string` | Title displayed at the top of the window |
| `StartupSound.Toggle` | `boolean` | Whether to play a sound on open |
| `StartupSound.SoundID` | `number \| nil` | Roblox asset ID for the sound |
| `StartupSound.TimePosition` | `number` | Playback start position in seconds |

---

### `tabHandler:Tab(name)`

Creates a new tab. Returns an `ElementHandler` object for adding elements to that tab.

```lua
local Tab = Window:Tab("Settings")
```

---

### `tabHandler:Exit()`

Destroys the entire UI.

```lua
Window:Exit()
```

---

### Elements

All element methods are called on the `ElementHandler` returned by `:Tab()`.

---

#### `:Label(text)`

Displays a non-interactive text label.

```lua
Tab:Label("Section Header")
```

| Parameter | Type | Description |
|---|---|---|
| `text` | `string` | Text to display |

---

#### `:Button(text, callback)`

A clickable button that fires a callback when pressed.

```lua
Tab:Button("Teleport", function()
    -- your code
end)
```

| Parameter | Type | Description |
|---|---|---|
| `text` | `string` | Button label |
| `callback` | `function` | Called on click |

---

#### `:Toggle(text, callback)`

A toggle switch. Fires the callback with the new boolean state on each click.

```lua
Tab:Toggle("God Mode", function(enabled)
    print(enabled) -- true or false
end)
```

| Parameter | Type | Description |
|---|---|---|
| `text` | `string` | Toggle label |
| `callback` | `function(state: boolean)` | Called with the new state |

---

#### `:Slider(text, default, min, max, callback)`

A horizontal slider for numeric input.

```lua
Tab:Slider("Walk Speed", 16, 0, 100, function(value)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
end)
```

| Parameter | Type | Description |
|---|---|---|
| `text` | `string` | Slider label |
| `default` | `number` | Initial display value |
| `min` | `number` | Minimum value |
| `max` | `number` | Maximum value |
| `callback` | `function(value: number)` | Called continuously while dragging |

> **Note:** The callback fires on every `RenderStepped` frame while the mouse is held. Keep callbacks lightweight to avoid performance issues.

---

#### `:Textbox(text, callback)`

A text input field. Fires the callback when the user finishes typing and unfocuses.

```lua
Tab:Textbox("Player Name", function(input)
    print("Entered:", input)
end)
```

| Parameter | Type | Description |
|---|---|---|
| `text` | `string` | Label shown beside the input |
| `callback` | `function(text: string)` | Called on `FocusLost` |

---

## Full Example

```lua
local Library = require(path.to.Library)

local Window = Library:Create({
    Name = "Exploit Menu",
    StartupSound = {
        Toggle = true,
        SoundID = 6958727243,
        TimePosition = 0
    }
})

-- Tab 1: Player
local PlayerTab = Window:Tab("Player")

PlayerTab:Label("Movement")

PlayerTab:Slider("Walk Speed", 16, 0, 200, function(v)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = v
end)

PlayerTab:Slider("Jump Power", 50, 0, 200, function(v)
    game.Players.LocalPlayer.Character.Humanoid.JumpPower = v
end)

PlayerTab:Toggle("Infinite Jump", function(state)
    _G.InfiniteJump = state
end)

-- Tab 2: Misc
local MiscTab = Window:Tab("Misc")

MiscTab:Textbox("Teleport to Player", function(name)
    -- teleport logic
end)

MiscTab:Button("Exit Menu", function()
    Window:Exit()
end)
```

---

## Notes

- The library parents itself to `game.CoreGui` and auto-removes any previous `dark_UI` instance on load.
- The window is draggable by clicking and holding anywhere on the main frame.
- The first tab added is automatically made visible.
- All default parameter values fall back gracefully if `nil` is passed (e.g. `Button` defaults to `"Button"` and a no-op callback).
