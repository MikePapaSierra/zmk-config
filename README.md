# Corne LP ZMK configuration

Personal ZMK configuration for a **Corne LP** with **nice!nano v2** controllers.

The layout is intentionally kept close to a regular **QWERTY laptop keyboard** so switching back to a non-programmable keyboard is easy. It is meant for **Polish and English** typing, with host-side language switching and keyboard-side shortcuts for symbols, navigation, and device selection.

## Hardware target

This repository builds firmware for:

- `nice_nano//zmk + corne_left`
- `nice_nano//zmk + corne_right`

The build matrix is defined in `build.yaml`.

`nice_nano//zmk` is the current ZMK board target for the nice!nano v2 hardware used by this keyboard on ZMK `main`.

## Key features

- Laptop-friendly QWERTY base layer
- `RAlt` on thumb for AltGr-based host layouts
- Left outer bottom key is a mod-tap:
  - **tap:** `LAlt`
  - **hold:** `LCtrl`
- Mod-tap tapping term set to **300 ms**
- Separate **Lower** and **Raise** layers
- USB-only computer connection through the left half
- BLE retained only for communication between split halves
- OLED status for USB output, active layer, and typing speed

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
|      |      |      |      |      |      |   | HOME | PGDN | PGUP | END  |      | PIPE |
|LCTRL |      |      |      |      |      |   |  -   |  =   |      |      |  \   |  `   |
                +------+------+------+           +------+------+------+
                | LGUI |      | SPC  |           | RET  |      | RALT |
                +------+------+------+           +------+------+------+
```

Raise is the number/navigation layer.

## Display and power

Enabled in `config/corne.conf`:

- OLED display
- active layer widget
- USB output widget
- typing-speed (WPM) widget

Backlight, battery reporting, external-power control, idle timeout, and deep sleep are disabled.

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

## Split and output behavior

- Connect the USB cable to the left (central) half; it is the only computer-facing keyboard device.
- The supplied TRRS cable powers the right half.
- BLE remains active only for internal communication between the halves. There are no Bluetooth
  profile, bond-clear, or output-switching bindings, so keyboard input is always sent over USB.
