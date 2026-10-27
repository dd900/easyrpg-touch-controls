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

The touch controls are intended for touch-capable mobile browsers. When a touchscreen is detected and no gamepad is connected, the controls are shown. They are hidden in non-touch browser environments and while a gamepad is connected.

* **Joystick:** Move in a direction to send the corresponding arrow-key input. Diagonal movement sends two arrow keys.
* **D-pad mode:** Use the D-PAD button in the utility group to switch between the joystick and four separate directional buttons.
* **Action buttons:** Send Shift, Escape, and Enter.
* **Shortcut buttons:** Send F1, F12, F, and G.
* **Numpad:** Select NUM + to open the keypad, then select it again to close it.
* **Fullscreen:** Toggle fullscreen where the browser supports the Fullscreen API.

Buttons support pointer input, including touch and mouse. The controls also suppress browser scrolling and long-press behavior while being used.

## Installation

1. Back up your existing `index.html`.
2. Replace it with this repository's `index.html` in the EasyRPG Player web folder.
3. Open or reload the player in a touch-capable mobile browser and test the controls.

The location of `index.html` depends on your EasyRPG installation. Keep the bundled player files and game data in place; this customization is for the HTML wrapper.

**Back up your original file before replacing it.** Updates to the player may overwrite your modifications. Consider keeping the customized file in a separate location or using a bind mount where supported.

## Control Layout Customization

Edit the CSS variables at the beginning of the `<style>` block in `index.html`. Each group can be moved and scaled independently, with separate values for portrait and landscape. The groups are:

| Group | Controls |
| --- | --- |
| `utility` | Shortcut buttons and D-pad mode toggle |
| `dpad` | Joystick or four-way D-pad |
| `action` | Shift, Escape, and Enter |
| `numpad` | Numpad toggle and keypad |

The fullscreen button stays at the top-left of the screen and is not part of an adjustable group.

Each group has six variables: portrait `x`, `y`, and `scale`, and landscape `x`, `y`, and `scale`. The variable pattern is `--<group>-group-<orientation>-<setting>`. For example, `--dpad-group-portrait-x` controls the D-pad's horizontal offset in portrait. The default offsets are `0px` and the default scale is `1` for every group and orientation.

For example, to move the portrait D-pad 12 pixels right and 8 pixels up, and make it 10% larger, set:

```css
--dpad-group-portrait-x: 12px;
--dpad-group-portrait-y: -8px;
--dpad-group-portrait-scale: 1.1;
```

Positive `x` moves a group right; positive `y` moves it down. Negative values move it left or up. Offsets accept CSS lengths such as `px` or `rem`. A scale of `1` keeps the default size, a value above `1` enlarges the whole group, and a value between `0` and `1` shrinks it. Scale values have no unit. Use the `-landscape-` variables to set different adjustments for landscape; the page switches between the two sets automatically when its orientation changes.

Groups stay anchored to their existing screen edges. Scaling expands or shrinks from the group's anchor: top-left for utility, bottom-left for D-pad, bottom-right for action, and top-right for numpad. Adjust the offsets if a larger or smaller group needs repositioning. Each group keeps its existing internal arrangement and spacing; there are no per-gap settings.

The controls also have responsive base sizes. The `--controls-size-portrait` and `--controls-size-landscape` variables near the same section set the sizing basis used by the controls; their minimum and maximum values are controlled by the corresponding `--controls-size-min`, `--controls-size-max`, `--controls-size-landscape-min`, and `--controls-size-landscape-max` variables. For most adjustments, use the group's `scale` setting so only that group changes size.

## Compatibility

Designed for the EasyRPG Player in mobile browsers. Touch behavior and fullscreen support vary by browser and device. The page uses standard pointer events and CSS viewport sizing; compatibility may also depend on the EasyRPG Player version.

## Troubleshooting

* **Controls are not visible:** Open the page on a touch-capable device or browser. A connected gamepad hides the touch controls; disconnect it to show them again.
* **Fullscreen does not activate:** Some browsers restrict fullscreen requests. Use the browser's own fullscreen option if the button is unavailable.
* **A layout adjustment is not taking effect:** Check that you edited the variable for the active orientation (`portrait` or `landscape`) and used a CSS length unit for `x` and `y`.

## Disclaimer

This is a community customization and is not an official EasyRPG project.
