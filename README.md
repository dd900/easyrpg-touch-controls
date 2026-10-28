# EasyRPG Touch Controls

Custom mobile touch controls for the EasyRPG player, designed to make RPG Maker games more comfortable to play on phones and tablets.

## Features

* Virtual joystick with an optional four-way D-pad mode
* On-screen action buttons for Shift, Escape, and Enter
* Shortcut buttons for F1, F12, F, and G
* Expandable on-screen numpad
* Fullscreen control
* Separate responsive layouts for portrait and landscape

## Controls

The touch controls are intended for touch-capable browsers. When a touchscreen is detected and no gamepad is connected, the controls are shown. They are hidden in non-touch browser environments and while a gamepad is connected. The fullscreen button remains available independently.

* **Joystick:** Move in a direction to send the corresponding arrow-key input. Diagonal movement sends two arrow keys.
* **D-pad mode** (full version only): Use the D-PAD button in the utility group to switch between the joystick and four directional controls. D-pad mode sends one direction at a time.
* **Action buttons:** Send Shift, Escape, and Enter.
* **Shortcut buttons:** Send F1, F12, F, and G.
* **Numpad** (versions with a numpad): Select NUM + to open the keypad, then select it again to close it.
* **Fullscreen:** Toggle fullscreen where the browser supports the Fullscreen API.

Buttons support pointer input, including touch and mouse. The controls also suppress browser scrolling and long-press behavior while being used.
The directional control can also be focused and operated with the arrow keys.

## Versions

The full `index.html` has a joystick with an optional D-pad mode and a numpad. If you only need part of that, pick one of the smaller versions in the `variants` folder:

| File | Directional control | Numpad |
| --- | --- | --- |
| `index.html` | Joystick, with a toggle for four-way D-pad mode | Yes |
| `variants/index.dpad.html` | Four-way D-pad only (no toggle) | Yes |
| `variants/index.joystick.html` | Joystick only (no toggle) | Yes |
| `variants/index.dpad-no-numpad.html` | Four-way D-pad only (no toggle) | No |
| `variants/index.joystick-no-numpad.html` | Joystick only (no toggle) | No |

The variants have the unused controls, styles, variables, and code removed. Without the D-pad mode toggle, the shortcut buttons form a 2x2 grid (F1, F12, F, G). All versions are generated from one source file; see [Development](#development).

## Installation

1. Back up your existing `index.html`.
2. Choose a version from the table above and copy it into the EasyRPG Player web folder. A file from the `variants` folder must be renamed to `index.html`.
3. Open or reload the player in a touch-capable mobile browser and test the controls.

The location of `index.html` depends on your EasyRPG installation. Keep the bundled player files and game data in place; this customization is for the HTML wrapper.

**Back up your original file before replacing it.** Updates to the player may overwrite your modifications. Consider keeping the customized file in a separate location or using a bind mount where supported.

## Control Layout Customization

Edit the CSS variables at the beginning of the `<style>` block in `index.html`. The variables for controls a version does not include (for example `--numpad-*` or `--joystick-*`) only exist in the versions that have those controls. Each group can be moved and scaled independently, with separate values for portrait and landscape. The groups are:

| Group | Controls |
| --- | --- |
| `utility` | Shortcut buttons and D-pad mode toggle |
| `directional` | Joystick or four-way D-pad |
| `action` | Shift, Escape, and Enter |
| `numpad` | Numpad toggle and keypad |

The fullscreen button stays at the top-left of the screen and is not part of an adjustable group. Its position is set with the inset variables below.

Each group has six variables: portrait `x`, `y`, and `scale`, and landscape `x`, `y`, and `scale`. The variable pattern is `--<group>-group-<orientation>-<setting>`. For example, `--directional-group-portrait-x` controls the joystick or D-pad's horizontal offset in portrait. The default offsets are `0px` and the default scale is `1` for every group and orientation.

For example, to move the portrait joystick or D-pad 12 pixels right and 8 pixels up, and make it 10% larger, set:

```css
--directional-group-portrait-x: 12px;
--directional-group-portrait-y: -8px;
--directional-group-portrait-scale: 1.1;
```

Positive `x` moves a group right; positive `y` moves it down. Negative values move it left or up. Offsets accept CSS lengths such as `px` or `rem`. A scale of `1` keeps the default size, a value above `1` enlarges the whole group, and a value between `0` and `1` shrinks it. Scale values have no unit. Use the `-landscape-` variables to set different adjustments for landscape; the page switches between the two sets automatically when its orientation changes.

Groups stay anchored to their existing screen edges. Scaling expands or shrinks from the group's anchor: top-left for utility, bottom-left for directional, bottom-right for action, and top-right for numpad. Adjust the offsets if a larger or smaller group needs repositioning. On short landscape screens, shortcuts use a compact row and the open numpad extends to the left.

### Edge insets

Each group has its own inset variables, which set its distance from the screen edges. The safe area (camera cutout) is not taken into account, so controls can sit under it. If a notch covers a group, increase that group's inset.

| Variables | Moves |
| --- | --- |
| `--fullscreen-inset-top`, `--fullscreen-inset-left` | Fullscreen button |
| `--utility-inset-top`, `--utility-inset-left` | Shortcut buttons and D-pad mode toggle |
| `--shortcut-pad-offset-top` | Shortcut buttons, measured down from the top of the utility group |
| `--directional-inset-left`, `--directional-inset-bottom` | Joystick or D-pad |
| `--action-inset-right`, `--action-inset-bottom` | Shift, Escape, and Enter |
| `--numpad-inset-top`, `--numpad-inset-right` | Numpad toggle and keypad |

In landscape, `--fullscreen-inset-left`, `--utility-inset-left`, and `--shortcut-pad-offset-top` are overridden inside the landscape `@media` rule near the top of the `<style>` block. Edit them there for landscape.

### Sizes

The controls have responsive base sizes. The `--base-size-portrait` and `--base-size-landscape` variables set the sizing basis shared by all groups; their minimum and maximum values are controlled by `--base-size-min`, `--base-size-max`, `--base-size-landscape-min`, and `--base-size-landscape-max`. For most adjustments, use the group's `scale` setting so only that group changes size.

Individual sizes are derived from the base size and can be overridden in the same place:

| Variable | Controls |
| --- | --- |
| `--action-button-size` | Diameter of the Shift, Escape, and Enter buttons |
| `--directional-size` | Overall size of the joystick or D-pad area |
| `--joystick-base-size`, `--joystick-knob-size`, `--joystick-knob-color` | Joystick base and knob; sizes are relative to the area |
| `--dpad-size` | Four-way D-pad cross; relative to the area |
| `--shortcut-base-size`, `--shortcut-button-width`, `--shortcut-button-height` | Shortcut buttons |
| `--numpad-key-size` | Numpad keys |
| `--control-gap` | Spacing between buttons in the shortcut pad and numpad |

The `--<group>-current-*` variables are internal; do not change them.

## Compatibility

Designed for the EasyRPG Player in mobile browsers. Touch behavior and fullscreen support vary by browser and device. The page uses standard pointer events and CSS viewport sizing; compatibility may also depend on the EasyRPG Player version.

## Troubleshooting

* **Controls are not visible:** Open the page on a touch-capable device or browser. A connected gamepad hides the touch controls; disconnect it to show them again.
* **Fullscreen does not activate:** Some browsers restrict fullscreen requests. Use the browser's own fullscreen option if the button is unavailable.
* **A layout adjustment is not taking effect:** Check that you edited the variable for the active orientation (`portrait` or `landscape`) and used a CSS length unit for `x` and `y`.

## Development

`index.html` and every file in `variants` are generated from `src/index.template.html` by `build.js`. Edit the template, never the generated files, then run [Node.js](https://nodejs.org) (no packages needed):

```sh
node build.js          # regenerate all versions
node build.js --check  # fail if any generated file is out of date
```

The template is the full version with some parts wrapped in directives, each on its own line as an HTML, CSS, or JS comment: `<!-- @if numpad -->`, `/* @if numpad */`, or `// @if numpad`. `@elif`, `@else`, and `@endif` work the same way. A condition combines these flags with `!`, `&&`, and `||`:

| Flag | True when the version has |
| --- | --- |
| `joystick` | The joystick |
| `dpad` | The four-way D-pad |
| `toggle` | Both, and so the D-PAD mode button |
| `numpad` | The numpad |

Placeholders such as `{{shortcutButtons}}` are replaced with a per-version value. To add a version, add a line for it to `OUTPUTS` in `build.js`.

## Disclaimer

This is a community customization and is not an official EasyRPG project.
