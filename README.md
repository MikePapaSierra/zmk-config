# Corne LP ZMK configuration

Personal ZMK configuration for a **Corne LP** with **nice!nano v2** controllers.

The layout is intentionally kept close to a regular **QWERTY laptop keyboard** so switching back to a non-programmable keyboard is easy. It is meant for **Polish and English** typing, with host-side language switching and keyboard-side shortcuts for symbols, navigation, and device selection.

## Hardware target

This repository builds firmware for:

- `nice_nano_v2 + corne_left`
- `nice_nano_v2 + corne_right`

The build matrix is defined in `build.yaml`.

## Key features

- Laptop-friendly QWERTY base layer
- `RAlt` on thumb for AltGr-based host layouts
- Left outer bottom key is a mod-tap:
  - **tap:** `LAlt`
  - **hold:** `LCtrl`
- Mod-tap tapping term set to **300 ms**
- Separate **Lower** and **Raise** layers
- Five Bluetooth profiles
- Explicit **USB** / **BLE** output switching
- OLED status for battery, layer, and output

## Layer diagrams

### Base layer

```text
+------+------+------+------+------+------+   +------+------+------+------+------+------+
| TAB  |  Q   |  W   |  E   |  R   |  T   |   |  Y   |  U   |  I   |  O   |  P   | BSPC |
|LSHFT |  A   |  S   |  D   |  F   |  G   |   |  H   |  J   |  K   |  L   |  ;   |  '   |
|CTL/AL|  Z   |  X   |  C   |  V   |  B   |   |  N   |  M   |  ,   |  .   |  /   |  `   |
                +------+------+------+           +------+------+------+
                | LGUI | LWR  | SPC  |           | RET  | RSE  | RALT |
                +------+------+------+           +------+------+------+
```

Notes:

- `CTL/AL` means **hold for Left Ctrl, tap for Left Alt**
- `RAlt` is useful for Polish Programmer-style host layouts

### Lower layer

```text
+------+------+------+------+------+------+   +------+------+------+------+------+------+
| ESC  |  !   |  @   |  #   |  $   |  %   |   |  ^   |  &   |  *   |  (   |  )   | DEL  |
|LSHFT |      |      |      |  {   |  }   |   | LEFT | DOWN |  UP  | RIGHT|  :   |  "   |
|LCTRL |      |      |      |  [   |  ]   |   |  _   |  +   |  <   |  >   |  ?   |  ~   |
                +------+------+------+           +------+------+------+
                | LGUI |      | SPC  |           | RET  |      | RALT |
                +------+------+------+           +------+------+------+
```

Lower is the symbol/navigation layer.

### Raise layer

```text
+------+------+------+------+------+------+   +------+------+------+------+------+------+
| TAB  |  1   |  2   |  3   |  4   |  5   |   |  6   |  7   |  8   |  9   |  0   | BSPC |
|BTCLR | USB  | BLE  |      |      |      |   | HOME | PGDN | PGUP | END  |      | PIPE |
|LCTRL | BT1  | BT2  | BT3  | BT4  | BT5  |   |  -   |  =   |      |      |  \   |  `   |
                +------+------+------+           +------+------+------+
                | LGUI |      | SPC  |           | RET  |      | RALT |
                +------+------+------+           +------+------+------+
```

Raise is the number/system layer.

Notes:

- `BT1` to `BT5` select Bluetooth profiles
- `BTCLR` clears the current Bluetooth bond
- `USB` forces output over USB
- `BLE` forces output over Bluetooth

## Display and power

Enabled in `config/corne.conf`:

- OLED display
- battery widget
- active layer widget
- active output widget
- external power support

Backlight is disabled.

## Build

GitHub Actions is configured in `.github/workflows/build.yml` and builds this config on:

- `push`
- `pull_request`
- `workflow_dispatch`

To build firmware:

1. Push changes to GitHub.
2. Open the latest workflow run.
3. Download the generated artifacts for `corne_left` and `corne_right`.

## Flashing

Flash each half with its matching artifact:

- left half -> `corne_left`
- right half -> `corne_right`

Typical nice!nano flashing flow:

1. Put the controller into bootloader mode.
2. Mount the board as a USB drive.
3. Copy the matching `.uf2` file onto the drive.

## Bluetooth and output behavior

- Bluetooth profile selection is persistent
- If you clear a bond, also remove the old pairing from the host
- Output switching is useful when USB is used only for power but typing should still go over Bluetooth
