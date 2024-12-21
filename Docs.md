# Linoria Documentation

## Overview
Linoria is a modular library designed for creating dynamic, configurable UI elements in Roblox scripts. This library provides a robust set of tools and components to construct user interfaces programmatically, with TypeScript typings for added type safety.

This documentation explains how the Linoria library works, its structure, and how to use it effectively.

---

## Installation
To use Linoria, you need to import the library and its associated components. Linoria is dynamically loaded using the `loadstring` function to fetch Lua scripts from a remote repository. For type safety, it integrates with TypeScript through typings.

```typescript
import {Linoria} from "pathtofile";
import { Elements, Library } from "pathtofile";

const repo = "https://raw.githubusercontent.com/scripts-ts/LinoriaLib/main/out/";

const {
    Builder,
    Window,
    Page,
    ThemeSection,
    ConfigSection,
    Groupbox,
    Tabbox,
    Tab,
    DependencyBox,
    Label,
    Toggle,
    Slider,
    Dropdown,
    MultiDropdown,
    Divider,
    Spacer,
    KeyPicker,
    ColorPicker,
} = loadstring(game.HttpGet(repo + "init.lua"))() as typeof Linoria;

const library = loadstring(game.HttpGet(repo + "library.lua"))() as Library;
const savemanager = loadstring(game.HttpGet(repo + "addons/savemanager.lua"))();
const thememanager = loadstring(game.HttpGet(repo + "addons/thememanager.lua"))();
```

---

## Structure

### Components
Linoria provides a wide range of components, each designed for a specific UI feature. The components include:

- **Builder**: Initializes and constructs UI elements.
- **Window**: Represents a UI window with customizable properties.
- **Page**: A container for grouping related UI elements.
- **ThemeSection**: Manages theme-related settings.
- **ConfigSection**: Handles configuration settings.
- **Groupbox**: Groups elements within a page.
- **Tabbox**: Manages multiple tabs.
- **Tab**: Represents an individual tab in a tabbed interface.
- **DependencyBox**: Displays elements conditionally based on dependencies.
- **Label**: Static text labels.
- **Toggle**: Switchable on/off states.
- **Slider**: Numeric input sliders.
- **Dropdown**: Single-choice dropdown menus.
- **MultiDropdown**: Multi-choice dropdown menus.
- **Divider**: Divides sections for better layout organization.
- **Spacer**: Adds spacing between elements.
- **KeyPicker**: Configures key bindings.
- **ColorPicker**: Provides color selection tools.

### Typings
Linoria integrates with TypeScript for better type safety. The `typeof Linoria` type describes the structure of the module being dynamically loaded.

```typescript
import {Linoria} from "pathtofile";

const {
    Builder,
    Window,
    Page,
    ...
} = loadstring(game.HttpGet(repo + "init.lua"))() as typeof Linoria;
```

The `typeof Linoria` typing ensures that components dynamically loaded at runtime conform to the expected structure. This prevents misuse and enables code autocompletion and type checking in editors.

---

## Usage

### Creating a Basic UI
```typescript
const window = new Builder()
    .createWindow({
        Title: "Example Window",
        Size: new Vector2(500, 600),
    });

const page = window.addPage({ Title: "Main Page" });
const groupbox = page.addGroupbox({ Title: "Settings" });

groupbox.addToggle({
    Title: "Enable Feature",
    Default: false,
    Callback: (state) => {
        print(`Feature enabled: ${state}`);
    },
});
```

### Adding a Key Picker
```typescript
const keyPicker = groupbox.addKeyPicker({
    Default: Enum.KeyCode.F,
    Mode: "Hold",
    Callback: (isPressed) => {
        print(`Key is ${isPressed ? "pressed" : "released"}`);
    },
});
```

### Using Dependencies
```typescript
const dependencyBox = groupbox.addDependencyBox({
    DependsOn: "Enable Feature",
    Value: true,
    Elements: [
        new Label({ Text: "This depends on the toggle being active." })
    ],
});
```

---

## Addons

### SaveManager
The `savemanager` addon handles saving and loading configurations.

```typescript
savemanager.SetLibrary(library);
savemanager.LoadFromFile("config.json");
savemanager.SaveToFile("config.json");
```

### ThemeManager
The `thememanager` addon allows theme customization.

```typescript
thememanager.SetLibrary(library);
thememanager.ApplyTheme("DarkTheme");
```

---

## Runtime Considerations
- **Dynamic Loading**: The `loadstring` function fetches and executes Lua scripts at runtime. Ensure the repository URLs are accessible and the returned data adheres to the expected structure.
- **Error Handling**: Since TypeScript does not validate runtime structures, mismatches between typings and actual data can result in errors.
- **Version Control**: Use specific versions of the repository to avoid breaking changes.

---

## Additional Resources
- [GitHub Repository](https://github.com/scripts-ts/LinoriaLib)
- [Original Linoria UI](https://github.com/violin-suzutsuki/LinoriaLib)

---

This documentation should help you understand and utilize Linoria effectively in your Roblox projects.

