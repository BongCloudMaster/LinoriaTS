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
new Builder()
	.root("brickmane_hub", "sexy jade")
	.library(library)
	.withSaveManager(savemanager)
	.withThemeManager(thememanager)
	.windows([
		new Window()
			.title("Brickmane Hub | sexy jade")
			.centered(true)
			.autoShow(true)
			.withFadeTime(0)
			.pages([new Page().title("Gameplay").left([new Groupbox().title("Silent Aim SEXY!!11")])]),
	])
	.renderUI();

```

### Adding a Toggle
```typescript
declare const Toggles: Toggles;

new Builder()
	.root("brickmane_hub", "sexy jade")
	.library(library)
	.withSaveManager(savemanager)
	.withThemeManager(thememanager)
	.windows([
		new Window()
			.title("Brickmane Hub | sexy jade")
			.centered(true)
			.autoShow(true)
			.withFadeTime(0)
			.pages([
				new Page()
					.title("Gameplay")
					.left([
						new Groupbox()
							.title("Silent Aim SEXY!!11")
							.elements([
								new Toggle("gameplay.ranged.silentaim")
									.title("Enabled")
									.tooltip("shoots silently (WOOWWW NO SHIT!)")
									.default(false),
							]),
					]),
			]),
	])
	.renderUI();

interface Toggles {
	"gameplay.ranged.silentaim": Elements.Toggle;
}
```

### Adding a Key Picker
```typescript
declare const Toggles: Toggles;
declare const Options: Options;

new Builder()
	.root("brickmane_hub", "sexy jade")
	.library(library)
	.withSaveManager(savemanager)
	.withThemeManager(thememanager)
	.windows([
		new Window()
			.title("Brickmane Hub | sexy jade")
			.centered(true)
			.autoShow(true)
			.withFadeTime(0)
			.pages([
				new Page().title("Gameplay").left([
					new Groupbox().title("Silent Aim SEXY!!11").elements([
						new Toggle("gameplay.ranged.silentaim")
							.title("Enabled")
							.tooltip("shoots silently (WOOWWW NO SHIT!)")
							.default(false)
							.extensions([
								new KeyPicker("gameplay.ranged.silentaim_key")
									.title("Silent Aim")
									.bind("T")
									.mode("Hold"),
							]),
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

### Using Dependencies
```typescript
declare const Toggles: Toggles;
declare const Options: Options;

new Builder()
	.root("brickmane_hub", "sexy jade")
	.library(library)
	.withSaveManager(savemanager)
	.withThemeManager(thememanager)
	.windows([
		new Window()
			.title("Brickmane Hub | sexy jade")
			.centered(true)
			.autoShow(true)
			.withFadeTime(0)
			.pages([
				new Page().title("Gameplay").left([
					new Groupbox().title("Silent Aim SEXY!!11").elements([
						new Toggle("gameplay.ranged.silentaim")
							.title("Enabled")
							.tooltip("shoots silently (WOOWWW NO SHIT!)")
							.default(false)
							.extensions([
								new KeyPicker("gameplay.ranged.silentaim_key")
									.title("Silent Aim")
									.bind("T")
									.mode("Hold"),
							]),
						new DependencyBox()
							.dependsOn("gameplay.ranged.silentaim", true)
							.elements([
								new Toggle("gameplay.ranged.silentaimdebugger")
									.title("Debugger")
									.tooltip("Enable debug notifications for Silent Aim")
									.default(true),
							]),
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

### Adding a new page
To add a new page, simply append a `new Page()` instance to the pages array:

```typescript
new Builder()
	.root("brickmane_hub", "sexy jade")
	.library(library)
	.withSaveManager(savemanager)
	.withThemeManager(thememanager)
	.windows([
		new Window()
			.title("Brickmane Hub | sexy jade")
			.centered(true)
			.autoShow(true)
			.withFadeTime(0)
			.pages([
				new Page().title("Gameplay").left([
					new Groupbox().title("Silent Aim SEXY!!11").elements([
						new Toggle("gameplay.ranged.silentaim")
							.title("Enabled")
							.tooltip("shoots silently (WOOWWW NO SHIT!)")
							.default(false)
							.extensions([
								new KeyPicker("gameplay.ranged.silentaim_key")
									.title("Silent Aim")
									.bind("T")
									.mode("Hold"),
							]),
						new DependencyBox()
							.dependsOn("gameplay.ranged.silentaim", true)
							.elements([
								new Toggle("gameplay.ranged.silentaimdebugger")
									.title("Debugger")
									.tooltip("Enable debug notifications for Silent Aim")
									.default(true),
							]),
					]),
				]),
				new Page().title("IdkIforgot").left([
					new Groupbox().title("Misc WHAT THE FUCK DOES THIS DO!!!/??").elements([
						new Toggle("gameplay.ranged.auto_parry")
							.title("Enabled")
							.tooltip("auto parries the enemies attacks (WOOWWW NO SHIT!)")
							.default(false)
							.extensions([
								new KeyPicker("gameplay.ranged.auto_parrykey")
									.title("Auto Parry")
									.bind("F")
									.mode("Hold"),
							]),
						new DependencyBox()
							.dependsOn("gameplay.ranged.auto_parry", true)
							.elements([
								new Toggle("gameplay.ranged.auto_parrydebugger")
									.title("Debugger")
									.tooltip("Enable debug notifications for Auto Parry")
									.default(true),
							]),
					]),
				]),
			]),
	])
	.renderUI();
```


### Final Touches
This is what it looks like when you are done
```typescript
import { Elements, Library } from "../../Linoria/library";
import { Linoria } from "../../Linoria/linoria";

declare const Toggles: Toggles;
declare const Options: Options;

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
} = loadstring(game.HttpGet(repo + "init.lua"))() as typeof Linoria; // Dynamically load Linoria and assert its type
const library = loadstring(game.HttpGet(repo + "library.lua"))() as Library; // Load the library with a type assertion
const savemanager = loadstring(game.HttpGet(repo + "addons/savemanager.lua"))();
const thememanager = loadstring(game.HttpGet(repo + "addons/thememanager.lua"))();

new Builder()
	.root("brickmane_hub", "sexy jade")
	.library(library)
	.withSaveManager(savemanager)
	.withThemeManager(thememanager)
	.windows([
		new Window()
			.title("Brickmane Hub | sexy jade")
			.centered(true)
			.autoShow(true)
			.withFadeTime(0)
			.pages([
				new Page().title("Gameplay").left([
					new Groupbox().title("Silent Aim SEXY!!11").elements([
						new Toggle("gameplay.ranged.silentaim")
							.title("Enabled")
							.tooltip("shoots silently (WOOWWW NO SHIT!)")
							.default(false)
							.extensions([
								new KeyPicker("gameplay.ranged.silentaim_key")
									.title("Silent Aim")
									.bind("T")
									.mode("Hold"),
							]),
						new DependencyBox()
							.dependsOn("gameplay.ranged.silentaim", true)
							.elements([
								new Toggle("gameplay.ranged.silentaimdebugger")
									.title("Debugger")
									.tooltip("Enable debug notifications for Silent Aim")
									.default(true),
							]),
					]),
				]),
				new Page().title("IdkIforgot").left([
					new Groupbox().title("Misc WHAT THE FUCK DOES THIS DO!!!/??").elements([
						new Toggle("gameplay.ranged.auto_parry")
							.title("Enabled")
							.tooltip("auto parries the enemies attacks (WOOWWW NO SHIT!)")
							.default(false)
							.extensions([
								new KeyPicker("gameplay.ranged.auto_parrykey")
									.title("Auto Parry")
									.bind("F")
									.mode("Hold"),
							]),
						new DependencyBox()
							.dependsOn("gameplay.ranged.auto_parry", true)
							.elements([
								new Toggle("gameplay.ranged.auto_parrydebugger")
									.title("Debugger")
									.tooltip("Enable debug notifications for Auto Parry")
									.default(true),
							]),
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
- [GitHub Repository](https://github.com/scripts-ts/LinoriaLib)
- [Original Linoria UI](https://github.com/violin-suzutsuki/LinoriaLib)

---

