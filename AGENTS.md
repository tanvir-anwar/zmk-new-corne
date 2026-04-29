# AGENTS.md

## Project Overview

ZMK firmware configuration for the **Eyelash Peripherals Corne** — a split ergonomic keyboard. This is **not** compatible with the standard foostan Corne and requires its own board definition.

Built on [ZMK Firmware](https://zmk.dev/) (v0.3.0) / Zephyr RTOS. The repo contains no traditional source code — it's devicetree overlays, Kconfig, and keymap definitions.

## Repository Structure

```
config/
  west.yml                  # West manifest — pulls ZMK and board module from GitHub
  eyelash_corne.keymap      # User keymap — layers, bindings, combos (primary edit target)
  eyelash_corne.conf        # Keyboard feature config (Bluetooth, sleep, RGB, etc.)
  eyelash_corne.json        # ZMK Studio layout metadata
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
4. **Build and flash** — compile firmware and flash both halves to test.
5. **Regenerate the diagram** — run keymap-drawer to update the SVG so it stays in sync.

The KEYMAP.md legend documents notation conventions (`hold/tap`, `[Sh/Caps]`, `<K1/K2>`, etc.) that map to specific ZMK behaviors. When adding new behavior types, update the legend first.

## Conventions

- The board definition exists locally in `boards/arm/eyelash_corne/` **and** is referenced as a remote module in `config/west.yml` (from `github.com/a741725193/zmk-new_corne`). The local copy takes precedence.
- Devicetree syntax (`.dtsi`, `.dts`, `.overlay`) — not C code. Use ZMK docs as reference, not general Zephyr docs.
- Keymap uses ZMK behavior bindings (e.g., `&kp`, `&mo`, `&lt`, `&bt`). See [ZMK keycodes docs](https://zmk.dev/docs/keymaps).
- Soft-off combo: Q + S + Z held for 2 seconds enters deep sleep. Wake via hardware reset button only.
