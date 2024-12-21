# Linoria Documentation

## Overview
Linoria is a modular library designed for creating dynamic, configurable UI elements in Roblox scripts. This library provides a robust set of tools and components to construct user interfaces programmatically, with TypeScript typings for added type safety.

This documentation explains how to use it effectively.

---

## Setup
Before using Linoria, ensure that you have the required dependencies installed. Use the following commands to install them via npm:

```bash
npm i @executor-ts/services@latest
npm i @executor-ts/types@latest
npm i -D executor-ts-transformer@latest
```

## `tsconfig.json` Setup
To ensure proper integration with Linoria and the required dependencies, modify your `tsconfig.json` file as follows:
Inside `typeRoots`, add:
```json
"typeRoots": ["node_modules/@rbxts", "node_modules/@executor-ts"]
```

Outside `compilerOptions`, add:
```json
"include": ["src", "node_modules/@rbxts", "node_modules/@executor-ts"]
```

Your complete `tsconfig.json` should look similar to this:
```json
{
	"compilerOptions": {
		// required
		"allowSyntheticDefaultImports": true,
		"downlevelIteration": true,
		"jsx": "react",
		"jsxFactory": "Roact.createElement",
		"jsxFragmentFactory": "Roact.createFragment",
		"module": "commonjs",
		"moduleResolution": "Node",
		"noLib": true,
		"resolveJsonModule": true,
		"experimentalDecorators": true,
		"forceConsistentCasingInFileNames": true,
		"moduleDetection": "force",
		"strict": true,
		"target": "ESNext",
		"typeRoots": ["node_modules/@rbxts", "node_modules/@executor-ts"],

		// configurable
		"rootDir": "src",
		"outDir": "out",
		"baseUrl": "src",
		"incremental": true,
		"tsBuildInfoFile": "out/tsconfig.tsbuildinfo",

		"plugins": [
			{
				"transform": "executor-ts-transformer"
			}
		]
	},
	"include": ["src", "node_modules/@rbxts", "node_modules/@executor-ts"]
}
```

---

## Installation

To use Linoria, you need to import the library and its associated components. By putting the folder into your project folder.

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

### Toggles and Options
## Overview
Linoria uses **Toggles** and **Options** to manage configurable UI settings dynamically. These represent states or preferences users can enable, disable, or customize.

## Toggles
A **Toggle** is used for binary settings (on/off). It includes properties like title, tooltip, and default state.

**Example:**
```typescript
declare const Toggles: Toggles;

new Toggle("gameplay.ranged.silentaim")
    .title("Enabled")
    .tooltip("Toggle silent aim functionality.")
    .default(false);

interface Toggles {
    "gameplay.ranged.silentaim": Elements.Toggle;
}
```

---

## Toggles
An **Option** is used for more advanced settings, such as key bindings or dropdown selections. Options extend components like `KeyPicker` or `Dropdown`.

**Example:**
```typescript
declare const Options: Options;

new KeyPicker("gameplay.ranged.silentaim_key")
    .title("Silent Aim")
    .bind("T")
    .mode("Hold");

interface Options {
    "gameplay.ranged.silentaim_key": Elements.KeyPicker;
}
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

declare const Toggles: Toggles;
declare const Options: Options;

new Builder()
    .root("hub_name", "UI Description")
    .library(library)
    .withSaveManager(savemanager)
    .withThemeManager(thememanager)
    .windows([
        new Window()
            .title("Hub Name | Description")
            .centered(true)
            .autoShow(true)
            .pages([
                new Page()
                    .title("Main Features")
                    .left([
                        new Groupbox().title("Gameplay").elements([
                            new Toggle("gameplay.ranged.silentaim")
                                .title("Silent Aim")
                                .default(false)
                                .extensions([
                                    new KeyPicker("gameplay.ranged.silentaim_key")
                                        .title("Silent Aim Key")
                                        .bind("T")
                                        .mode("Hold"),
                                ]),
                        ]),
                    ]),
                new Page()
                    .title("Settings")
                    .left([
                        new Groupbox().title("Advanced Options").elements([
                            new Toggle("settings.auto_save").title("Auto Save").default(true),
                        ]),
                    ]),
            ]),
    ])
    .renderUI();

interface Toggles {
    "gameplay.ranged.silentaim": Elements.Toggle;
}

interface Options {
    "gameplay.ranged.silentaim_key": Elements.KeyPicker;
}

```

---

## Additional Resources

- **GitHub Repository for TypeScript Version**: [scripts-ts/LinoriaLib](https://github.com/scripts-ts/LinoriaLib)  
  *Developed by [TerminalVibes](https://github.com/TerminalVibes), this version adds TypeScript typings and features to the original Linoria library for improved type safety and developer experience.*  

- **GitHub Repository for Original Lua Version**: [violin-suzutsuki/LinoriaLib](https://github.com/violin-suzutsuki/LinoriaLib)  
  *Created by [violin-suzutsuki](https://github.com/violin-suzutsuki), the original Linoria library provides a Lua-based framework for constructing Roblox user interfaces.*  

