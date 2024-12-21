# Linoria Documentation

## Overview
Linoria is a modular library designed for creating dynamic, configurable UI elements in Roblox scripts. This library provides a robust set of tools and components to construct user interfaces programmatically, with TypeScript typings for added type safety.

This documentation explains how the Linoria library works, its structure, and how to use it effectively.

---

## Installation

### Fetching the Library
Linoria is dynamically loaded using the `loadstring` function to fetch Lua scripts from a remote repository. Use the following code to fetch and initialize the library and its components:

```typescript
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

### Setting Up TypeScript Typings
For enhanced type safety, integrate Linoria's TypeScript typings into your project:

```typescript
import { Linoria } from "path-to-typings";
```

This ensures that all dynamically loaded components conform to their expected structures, enabling autocompletion and type checking in your editor.

---

## Structure

### Components
Linoria provides a wide range of components for building UIs:

- **Builder**: Initializes and constructs the UI structure.
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

---

## Usage

### Creating a Basic UI
The following example demonstrates how to create a basic UI using Linoria:

```typescript
new Builder()
    .root("my_hub", "default_theme")
    .library(library)
    .withSaveManager(savemanager)
    .withThemeManager(thememanager)
    .windows([
        new Window()
            .title("My Hub")
            .centered(true)
            .autoShow(true)
            .withFadeTime(0)
            .pages([
                new Page().title("Main Page").left([
                    new Groupbox().title("Features").elements([
                        new Toggle("features.example")
                            .title("Example Toggle")
                            .tooltip("An example toggle feature.")
                            .default(false),
                    ]),
                ]),
            ]),
    ])
    .renderUI();
```

### Adding Components

#### Toggle
A toggle is a switchable button for enabling or disabling a feature:

```typescript
new Toggle("features.example")
    .title("Example Feature")
    .tooltip("Enable or disable this feature.")
    .default(false);
```

#### Key Picker
A key picker binds specific keys to actions in your script:

```typescript
new KeyPicker("features.example_key")
    .title("Example Key")
    .bind("T")
    .mode("Hold");
```

#### Dependency Box
Use a dependency box to conditionally display elements:

```typescript
new DependencyBox()
    .dependsOn("features.example", true)
    .elements([
        new Toggle("features.example_debugger")
            .title("Enable Debugger")
            .tooltip("Show debug notifications.")
            .default(true),
    ]);
```

### Adding a New Page
To add a new page, append a `new Page()` instance to the pages array:

```typescript
new Page().title("Settings").left([
    new Groupbox().title("Options").elements([
        new Toggle("settings.auto_save")
            .title("Auto Save")
            .default(true),
    ]),
]);
```

---

## Full Example
Here is a complete example demonstrating Linoria's capabilities:

```typescript
const repo = "https://raw.githubusercontent.com/scripts-ts/LinoriaLib/main/out/";

const {
    Builder,
    Window,
    Page,
    Groupbox,
    Toggle,
    KeyPicker,
    DependencyBox,
} = loadstring(game.HttpGet(repo + "init.lua"))() as typeof Linoria;
const library = loadstring(game.HttpGet(repo + "library.lua"))() as Library;
const savemanager = loadstring(game.HttpGet(repo + "addons/savemanager.lua"))();
const thememanager = loadstring(game.HttpGet(repo + "addons/thememanager.lua"))();

new Builder()
    .root("example_hub", "default_theme")
    .library(library)
    .withSaveManager(savemanager)
    .withThemeManager(thememanager)
    .windows([
        new Window()
            .title("Example Hub")
            .centered(true)
            .autoShow(true)
            .withFadeTime(0)
            .pages([
                new Page().title("Features").left([
                    new Groupbox().title("Main Features").elements([
                        new Toggle("features.auto_parry")
                            .title("Auto Parry")
                            .tooltip("Automatically parry enemy attacks.")
                            .default(false)
                            .extensions([
                                new KeyPicker("features.auto_parry_key")
                                    .title("Parry Key")
                                    .bind("F")
                                    .mode("Hold"),
                            ]),
                        new DependencyBox()
                            .dependsOn("features.auto_parry", true)
                            .elements([
                                new Toggle("features.auto_parry_debugger")
                                    .title("Enable Debugger")
                                    .tooltip("Show debug notifications.")
                                    .default(true),
                            ]),
                    ]),
                ]),
            ]),
    ])
    .renderUI();
```

---

## Additional Resources
- [GitHub Repository](https://github.com/scripts-ts/LinoriaLib)
- [Original Linoria UI](https://github.com/violin-suzutsuki/LinoriaLib)

