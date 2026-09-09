# AGENTS.md

## Project Overview

ZMK firmware configuration for the **Eyelash Peripherals Corne** — a split ergonomic keyboard. This is **not** compatible with the standard foostan Corne and requires its own board definition.

Built on [ZMK Firmware](https://zmk.dev/) (v0.3.0) / Zephyr RTOS. The repo contains no traditional source code — it's devicetree overlays, Kconfig, and keymap definitions.

The repository also tracks personal layouts for the USB-only **Beekeeb 3W6HS**. Those layouts use [Vial](https://get.vial.today/) / QMK and are independent of the ZMK Corne firmware.

## Repository Structure

```
config/
  west.yml                  # West manifest — pulls ZMK and board module from GitHub
  eyelash_corne.keymap      # User keymap — layers, bindings, combos (primary edit target)
  eyelash_corne.conf        # Keyboard feature config (Bluetooth, sleep, RGB, etc.)
  eyelash_corne.json        # ZMK Studio layout metadata
3w6hs/
  factory-default.vil       # Untouched export of the stock 3W6HS Vial layout
  corne-inner5.vil          # Personal 3W6HS layout derived from the Corne inner columns
  corne-inner5.yaml         # Derived keymap-drawer input; not a second keymap source
  corne-inner5.svg          # Generated diagram for manual review
boards/arm/eyelash_corne/   # Board definition (also available as remote module via west.yml)
  eyelash_corne.dtsi        # Main devicetree include — pin mappings, peripherals
  eyelash_corne-layouts.dtsi # Physical layout definitions
  eyelash_corne_left.dts    # Left half devicetree
  eyelash_corne_right.dts   # Right half devicetree
  eyelash_corne_left_defconfig   # Left half Kconfig defaults
  eyelash_corne_right_defconfig  # Right half Kconfig defaults
  eyelash_corne.keymap       # Default/fallback keymap
  Kconfig.board              # Board Kconfig options
  Kconfig.defconfig          # Board Kconfig defaults
  board.cmake                # CMake flash/debug config
  eyelash_corne.yaml         # Board metadata
  eyelash_corne.zmk.yml      # ZMK module metadata
build.yaml                  # GitHub Actions build matrix (which boards/shields to compile)
keymap-drawer/              # Auto-generated keymap SVG diagrams
.github/workflows/
  build.yml                 # Firmware build workflow (produces .uf2 artifacts)
  draw.yml                  # Keymap diagram generation workflow
zephyr/module.yml           # Zephyr module registration
MANUAL.md                   # User manual / documentation
```

## Key Entry Points

- **Keymap customization**: `config/eyelash_corne.keymap` — where layers, key bindings, and combos are defined
- **Feature toggles**: `config/eyelash_corne.conf` — enable/disable Bluetooth, RGB, deep sleep, etc.
- **Build targets**: `build.yaml` — defines which board/shield combos to compile (left, right, studio, settings_reset)
- **Board hardware**: `boards/arm/eyelash_corne/eyelash_corne.dtsi` — pin mappings and hardware peripherals
- **3W6HS personal layout**: `3w6hs/corne-inner5.vil` — a loadable Vial layout; it is not ZMK firmware

## Build System

Firmware is built via **GitHub Actions** (`.github/workflows/build.yml`). The `build.yaml` matrix currently compiles:
- `eyelash_corne_right` + `nice_view` shield
- `eyelash_corne_left` + `nice_view` + ZMK Studio support
- `nice_nano_v2` + `settings_reset` (for resetting bond info)

Output: `.uf2` firmware files flashed via USB mass storage mode.

## Keymap Change Workflow

Keymap changes use a **markdown-first staging workflow**:

1. **Stage changes in `config/KEYMAP.md`** — edit the human-readable markdown tables to describe the desired layout. This is the design document; it's easier to review and reason about than raw devicetree syntax.
2. **Review the diff** — use `git diff config/KEYMAP.md` to verify the proposed changes make sense before touching firmware code.
3. **Update `config/eyelash_corne.keymap`** — translate the markdown tables into ZMK devicetree bindings. New behaviors (tap-dance, mod-morph, hold-tap) must be defined in the `behaviors {}` block before referencing them in layer bindings.
4. **Parse and generate the diagram** — validates the keymap syntax and catches errors faster than a full firmware build:
   ```bash
   source .venv/bin/activate
   keymap parse -z config/eyelash_corne.keymap > keymap-drawer/eyelash_corne.yaml
   keymap -c keymap_drawer.config.yaml draw keymap-drawer/eyelash_corne.yaml > keymap-drawer/eyelash_corne.svg
   ```
5. **Build and flash** — push to GitHub, download `.uf2` artifacts from the Actions workflow, and flash both halves.

The KEYMAP.md legend documents notation conventions (`hold/tap`, `[Sh/Caps]`, `<K1/K2>`, etc.) that map to specific ZMK behaviors. When adding new behavior types, update the legend first.

## 3W6HS Vial Layout Workflow

The 3W6HS is a USB-only, 36-key, 3×5+3 split. It has no Bluetooth, RGB,
encoder, joystick, sleep, or soft-off controls. Do not carry those Corne
actions into a 3W6HS layout.

1. **Use the `.vil` file as the source of truth** — keep
   `3w6hs/corne-inner5.vil` formatted with two-space JSON indentation. A
   `.vil` is loaded from Vial into the keyboard's dynamic-keymap storage; it
   is not firmware and must not be minified or flashed.
2. **Preserve the factory metadata** — start from
   `3w6hs/factory-default.vil` and retain its `uid`, protocol versions,
   physical matrix shape, settings, and unused feature slots. Only change
   intended key bindings, combos, or explicitly reviewed Vial settings.
3. **Keep the Corne correspondence explicit** — the 3W6HS has the Corne
   inner five columns (`L1`–`L5` and `R5`–`R1`) and six thumbs. Its matrix
   order is not physical reading order:
   - left fingers: rows 0–2, columns 0–4;
   - left thumbs, outer to inner: row 3, columns 2–4;
   - right fingers: rows 4–6, columns 0–4;
   - right thumbs, inner to outer: row 7, columns 0–2.
4. **Check firmware-backed behaviors** — a `.vil` can assign a QMK keycode,
   but it cannot enable a feature that the flashed Vial firmware omitted.
   Test `CW_TOGG` (Caps Word), `OSM(...)` (one-shot modifiers), and
   `KC_MS_BTN1`/`KC_MS_BTN2` (mouse clicks) on hardware after loading. If a
   feature is unavailable, the remedy is a separately authorized Vial/QMK
   firmware build and flash, not a different `.vil` encoding.
5. **Validate before loading** — confirm the JSON is valid and still has the
   factory's 10 layers, 8×10 matrix, UID, and protocol versions. Load the
   pretty-printed file directly in Vial and manually test every thumb,
   layer-tap, combo, and layer toggle.
6. **Generate a diagram, never hand-maintain a duplicate table** — use
   [Vial To Keymap Drawer](https://github.com/YAL-Tools/vial-to-keymap-drawer)
   to create an initial YAML rendering from the `.vil`, then retain a compact
   derived file at `3w6hs/corne-inner5.yaml`. Do not duplicate the keymap in
   `KEYMAP.md`.
   - The diagram YAML uses keymap-drawer's normalized physical layout:
     ```yaml
     layout:
       ortho_layout: {split: true, rows: 3, columns: 5, thumbs: 3}
     ```
     Its 36 key positions are physical reading order: left finger rows,
     right finger rows, left thumbs outer-to-inner, then right thumbs
     inner-to-outer. Do not use Vial's raw 8×10 matrix order as diagram
     positions.
   - The browser converter emits Vial's raw 8×10 matrix order. When the
     `.vil` changes, rerun the converter, then update the compact YAML by
     moving the 36 real keys into the physical order above and preserving
     readable display labels. Compare every changed YAML binding against the
     `.vil`; never treat the YAML as an independent source of truth.
   - Translate any Vial matrix-based combo positions into that 36-key order.
     The QWERTY ↔ Colemak-DH toggle is deliberately the **inner** thumb pair:
     sticky Shift plus `L2/Space`. In the `.vil`, its trigger is
     `OSM(MOD_LSFT)` + `LT2(KC_SPACE)` with output `TG(1)`; in the diagram
     YAML, its positions are `[32, 33]`. Do not move it to the outer
     `CMD/TAB` and `CTRL/ESC` thumbs. Show this combo on both `L0` and `L1`
     in the diagram because `TG(1)` toggles Colemak-DH on and off.
   - Render the tracked SVG with the repository's installed keymap-drawer:
     ```bash
     .venv/bin/keymap -c keymap_drawer.config.yaml draw \
       3w6hs/corne-inner5.yaml > 3w6hs/corne-inner5.svg
     ```
     Regenerate the compact YAML and SVG whenever `corne-inner5.vil` changes.
   - Visually inspect the SVG for thumb order, held layer-tap highlighting,
     layer-toggle combo placement, and all layer bindings. Ask the user to
     manually review it before committing.

## Coding Guidelines
1. ALWAYS use conventional commits syntax to write commit messages.
2. NEVER update `config/eyelash_corne.keymap` unless `config/KEYMAP.md` changes are manually reviewed and confirmed. The formatted 3W6HS `.vil` file is its own design source and does not require a duplicate `KEYMAP.md`.
3. ALWAYS ask the user to manually review and validate the rendered keymap diagram before the final commit.

## Conventions

- The board definition exists locally in `boards/arm/eyelash_corne/` **and** is referenced as a remote module in `config/west.yml` (from `github.com/a741725193/zmk-new_corne`). The local copy takes precedence.
- Devicetree syntax (`.dtsi`, `.dts`, `.overlay`) — not C code. Use ZMK docs as reference, not general Zephyr docs.
- Keymap uses ZMK behavior bindings (e.g., `&kp`, `&mo`, `&lt`, `&bt`). See [ZMK keycodes docs](https://zmk.dev/docs/keymaps).
- Soft-off combo: Q + S + Z held for 2 seconds enters deep sleep. Wake via hardware reset button only.
