# 3W6HS QMK/Vial Firmware Runbook

This document covers the separate QMK/Vial firmware workflow for the Beekeeb
3W6HS. It is only needed when a desired keycode depends on a QMK feature that
the keyboard's installed firmware does not include.

For ordinary remaps, edit the formatted
[`corne-inner5.vil`](corne-inner5.vil), regenerate its derived diagram, and
load that `.vil` directly in Vial. Do not build or flash firmware merely to
change a dynamic keymap binding.

## References

- [Beekeeb's 3W6HS Vial/QMK source, `vial` branch](https://github.com/beekeeb/vial-qmk-3w6hs/tree/vial)
- [Vendor 3W6HS keyboard README](https://github.com/beekeeb/vial-qmk-3w6hs/blob/vial/keyboards/beekeeb/3w6hs/readme.md)
- [QMK Caps Word](https://docs.qmk.fm/features/caps_word)
- [QMK RP2040 UF2 flashing](https://docs.qmk.fm/flashing#raspberry-pi-rp2040-uf2)
- [Vial desktop application](https://get.vial.today/)

The vendor source was checked on 2026-09-10 at `vial` commit
`d21104010e4b3812c39b32473a0b0b5ccd2af13c`.

## What a `.vil` can and cannot do

A `.vil` writes Vial's dynamic-keymap storage. It can assign keycodes and
Vial-supported settings, but it cannot compile an omitted QMK feature into the
firmware.

The currently useful split is:

| Change | Firmware build needed? |
| --- | --- |
| Rearrange normal keys, layers, hold-taps, one-shot modifiers, mouse buttons, or Vial settings | No — edit and load `corne-inner5.vil`. |
| Use `CW_TOGG` when Caps Word does not work | Yes — enable and flash Caps Word, then reload `corne-inner5.vil`. |
| Change the Vial keyboard UID, matrix, or number of dynamic layers | No — do not change these. It would make existing `.vil` backups incompatible. |

## Important invariants

The vendor Vial keymap has an 8×10 matrix and ten dynamic layers. Its
`VIAL_KEYBOARD_UID` is:

```c
{0xB3, 0xF5, 0x84, 0xE9, 0x9D, 0x7B, 0x1B, 0x50}
```

The checked-in `factory-default.vil` and `corne-inner5.vil` already match that
UID, matrix, and ten-layer limit. A custom firmware must retain all of these:

```c
// keyboards/beekeeb/3w6hs/keymaps/<keymap>/config.h
#define VIAL_KEYBOARD_UID {0xB3, 0xF5, 0x84, 0xE9, 0x9D, 0x7B, 0x1B, 0x50}
#define DYNAMIC_KEYMAP_LAYER_COUNT 10
```

It must also retain these Vial settings:

```make
# keyboards/beekeeb/3w6hs/keymaps/<keymap>/rules.mk
VIA_ENABLE = yes
VIAL_ENABLE = yes
VIAL_INSECURE = yes
```

`VIAL_INSECURE` is intentional in the vendor keymap: it permits Vial access
without a physical unlock key. Do not substitute the normal QMK upstream tree
for Beekeeb's `vial` branch; that branch contains the 3W6HS definition and
Vial integration used by this keyboard.

## One-time source and build setup

Keep the full QMK checkout outside this small configuration repository. That
prevents vendored QMK changes and generated build files from being committed
here.

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/).
   Docker supplies the QMK compiler and toolchain, so no local QMK, Homebrew,
   Python, or cross-compiler setup is needed.
2. Clone the exact Vial-enabled vendor source, including submodules:

   ```sh
   git clone --recurse-submodules --branch vial \
     https://github.com/beekeeb/vial-qmk-3w6hs.git \
     "$HOME/src/vial-qmk-3w6hs"
   cd "$HOME/src/vial-qmk-3w6hs"
   ```

3. If the checkout was cloned without submodules, initialize them:

   ```sh
   git submodule update --init --recursive
   ```

The vendor's Docker helper accepts a `keyboard:keymap[:target]` argument and
downloads the QMK build image when it is first run.

## Create a personal firmware keymap

Do not modify `keymaps/vial` in place. Copy it once into a personal keymap so
vendor updates remain easy to compare:

```sh
cd "$HOME/src/vial-qmk-3w6hs"
cp -R keyboards/beekeeb/3w6hs/keymaps/vial \
  keyboards/beekeeb/3w6hs/keymaps/personal
git switch -c personal-3w6hs
git add keyboards/beekeeb/3w6hs/keymaps/personal
git commit -m "feat(3w6hs): add personal Vial firmware keymap"
```

The compiled `keymap.c` is only the initial dynamic keymap. After flashing,
load `corne-inner5.vil` to establish the personal layout. Keep its `config.h`
and `vial.json` unchanged unless there is a deliberate, reviewed Vial protocol
change.

## Enable Caps Word

The current `CW_TOGG` binding in `corne-inner5.vil` cannot work until the
firmware includes Caps Word. Add this line to the copied keymap:

```make
# keyboards/beekeeb/3w6hs/keymaps/personal/rules.mk
CAPS_WORD_ENABLE = yes
```

No `keymap.c` edit is required: Vial can write the standard `CW_TOGG` keycode
into the dynamic keymap once the feature is compiled in.

The resulting `rules.mk` should be:

```make
VIA_ENABLE = yes
VIAL_ENABLE = yes
VIAL_INSECURE = yes
CAPS_WORD_ENABLE = yes
```

Commit this firmware-source change in the separate QMK checkout before
building. It is the durable record of what the flashed firmware supports.

## Build

Build the personal Vial keymap with Docker:

```sh
cd "$HOME/src/vial-qmk-3w6hs"
./util/docker_build.sh beekeeb/3w6hs:personal
```

The expected artifact is:

```text
.build/beekeeb_3w6hs_personal.uf2
```

Do not flash a `:default` build: it is not the Vial-enabled keymap. Do not
flash until the build succeeds and produces the `.uf2` named above.

## Back up, flash, and restore the personal layout

1. In Vial, use **File → Save current layout** and retain that backup outside
   the QMK checkout. The repository's `factory-default.vil` is the stock
   reference, while `corne-inner5.vil` is the intended personal layout.
2. Disconnect the keyboard from USB.
3. Hold the **BOOT** button — the top button on the keyboard's left side — and
   reconnect the USB cable. The RP2040 bootloader should mount a removable
   drive, commonly named `RPI-RP2`.
4. Copy `.build/beekeeb_3w6hs_personal.uf2` to that drive. The board should
   reboot automatically after the copy completes.
5. Open Vial and confirm it recognizes the keyboard.
6. Use **File → Load saved file** to load this repository's
   `corne-inner5.vil`. Do this even if the prior dynamic mapping appears to
   have survived the flash; do not rely on persistent dynamic-keymap storage.
7. Test both halves, every thumb key, Symbol and Nav/Fn layer selectors, mouse
   buttons, one-shot modifiers, and `CW_TOGG`.

Flashing one `.uf2` is sufficient for this keyboard's vendor firmware: it is a
single RP2040 keyboard with the split matrix managed by that firmware, not two
separately flashed wireless halves.

## Recovery and troubleshooting

- **No bootloader drive:** disconnect USB, hold the top-left BOOT button, and
  reconnect with a known data-capable cable.
- **Vial does not connect after flashing:** verify that the build target was
  `beekeeb/3w6hs:personal`, that the copied keymap still enables Vial, and
  that its UID, 8×10 matrix, and ten-layer count were preserved.
- **Caps Word still does nothing:** verify `CAPS_WORD_ENABLE = yes` is in
  `keymaps/personal/rules.mk`, rebuild the `personal` target, reflash, reload
  `corne-inner5.vil`, then test its `CW_TOGG` binding.
- **Need to return to stock behavior:** build and flash
  `./util/docker_build.sh beekeeb/3w6hs:vial`, then load
  `factory-default.vil` (or the Vial backup made before flashing).
