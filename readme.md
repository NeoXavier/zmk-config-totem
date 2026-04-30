<picture>
  <source media="(prefers-color-scheme: dark)" srcset="/docs/images/TOTEM_logo_dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="/docs/images/TOTEM_logo_bright.svg">
  <img alt="TOTEM logo font" src="/docs/images/TOTEM_logo_bright.svg">
</picture>

# ZMK CONFIG FOR THE TOTEM SPLIT KEYBOARD

[Here](https://github.com/GEIGEIGEIST/totem) you can find the hardware files and build guide.\
[Here](https://github.com/GEIGEIGEIST/qmk-config-totem) you can find the QMK config for the TOTEM.

TOTEM is a 38 key column-staggered split keyboard running [ZMK](https://zmk.dev/) or [QMK](https://docs.qmk.fm/). It's meant to be used with a SEEED XIAO BLE or RP2040.


![TOTEM layout](/docs/images/TOTEM_layout.svg)



## HOW TO USE

- fork this repo
- `git clone` your repo, to create a local copy on your PC (you can use the [command line](https://www.atlassian.com/git/tutorials) or [github desktop](https://desktop.github.com/))
- adjust the totem.keymap file (find all the keycodes on [the zmk docs pages](https://zmk.dev/docs/codes/))
- `git push` your repo to your fork
- on the GitHub page of your fork navigate to "Actions"
- scroll down and unzip the `firmware.zip` archive that contains the latest firmware
- connect the left half of the TOTEM to your PC, press reset twice
- the keyboard should now appear as a mass storage device
- drag'n'drop the `totem_left-seeeduino_xiao_ble-zmk.uf2` file from the archive onto the storage device
- repeat this process with the right half and the `totem_right-seeeduino_xiao_ble-zmk.uf2` file.

---

## PERSONAL CONFIG

This config uses a **USB dongle** setup: left half, right half, and a dongle — all **Seeeduino XIAO BLE**. The dongle is the BLE central (host); the two halves are peripherals.

### Important Files

| File | Purpose |
|---|---|
| `build.yaml` | Defines build targets — left, right, dongle, settings_reset |
| `config/totem.keymap` | **Main keymap** — edit this to change keys/layers |
| `config/totem.conf` | Global firmware settings (BLE power, pointing, logging) |
| `config/boards/shields/totem/totem_dongle.conf` | Dongle-specific config (2 peripherals, sleep) |
| `config/boards/shields/totem/totem_dongle.overlay` | Dongle GPIO pin assignments |
| `config/boards/shields/totem/totem_left/right.conf/.overlay` | Per-half configs |
| `Justfile` | Build task runner |

### Layers

| # | Name | Activated by |
|---|---|---|
| 0 | **Base** | Default (QWERTY) |
| 1 | **Num** | Hold `TAB` (left thumb) |
| 2 | **Sym** | Hold `ENTER` (right thumb) |
| 3 | **Func** | Hold `ESC` (outer left pinky) |
| 4 | **Movement** | Hold `'` (outer right pinky) |
| 5 | **BT** | Hold `F12` while in Func layer |

Home row mods on `A S D F` (and mirrored right): Shift / Ctrl / Alt / GUI.

### Local Build Workflow

```bash
# First-time setup
just init

# Build all targets
just build all

# Build a specific target (e.g. dongle only)
just build dongle

# Clean build cache
just clean

# Redraw keymap diagram
just draw
```

GitHub Actions (`.github/workflows/build.yml`) builds firmware on every push — grab `.uf2` files from CI artifacts without building locally.

### Changing Keys

Edit `config/totem.keymap`, find the layer you want, and update its `bindings = < ... >` block. Positions map to physical keys in row order (top→bottom, left→right). Then run `just build all` or push to trigger CI.
