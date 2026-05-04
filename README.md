# (Eyelash Peripherals) Corne ZMK Repository

### Components

1. Pre-soldered PCB and case - from [AliExpress Highland 3C Store](https://www.aliexpress.us/item/3256807477869827.html)
2. Akko Cilantro Tactile Switch - from [Amazon](https://www.amazon.com/dp/B0F2MQKHD1)
3. YMDK MX Keycaps - from [Amazon](https://www.amazon.com/dp/B07JKTQJQ7)

![image](corne-keyboard.jpg)

_[How I got here](https://www.reddit.com/r/ErgoMechKeyboards/comments/1t0zbf7/joining_the_club/)_

## Keymap Diagram Generation (Local)

Generate keymap diagrams locally to validate syntax before pushing.

### One-time setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install keymap-drawer
```

### Parse and draw

```bash
source .venv/bin/activate
keymap parse -z config/eyelash_corne.keymap > keymap-drawer/eyelash_corne.yaml
keymap -c keymap_drawer.config.yaml draw keymap-drawer/eyelash_corne.yaml > keymap-drawer/eyelash_corne.svg
```

Parsing catches syntax errors faster than a full firmware build.

### Quick remapping via ZMK Studio (no build needed)

For simple key binding changes, connect the left half via USB-C and open [ZMK Studio](https://zmk.studio/) in Chrome/Edge. Changes are applied live over USB — no tools or network access required.

## Firmware Build (GitHub Actions)

Firmware is compiled via GitHub Actions — local builds require a full [Zephyr SDK](https://docs.zephyrproject.org/latest/develop/getting_started/index.html) and ARM toolchain which are not included in this repo.

1. Push your changes to GitHub.
2. [Ensure the Actions workflow is enabled](https://docs.github.com/en/actions/managing-workflow-runs-and-deployments/managing-workflow-runs/disabling-and-enabling-a-workflow#enabling-a-workflow).
3. Download the `.uf2` firmware artifacts from the completed workflow run. The build produces three files:
   - `eyelash_corne_studio_left` — left half (with ZMK Studio support)
   - `eyelash_corne_right` — right half
   - `nice_nano_v2_settings_reset` — settings reset utility (see below)
4. Flash each half:
   1. Keep the power switch **ON**.
   2. **Double-tap the reset button** — the board enters bootloader mode and appears as a USB drive.
   3. Drag the correct `.uf2` file onto the drive. It flashes and reboots automatically.
   4. Repeat for the other half.

### Settings Reset

Flash `nice_nano_v2_settings_reset.uf2` to clear stored bonds, ZMK Studio overrides, and other saved settings. You'll need this when:

- **One half stops sending keypresses** — the BLE bond between halves is broken (most common after flashing new firmware to only one side).
- **Keys produce wrong output after a keymap change** — stale ZMK Studio overrides persist in flash and take priority over the compiled keymap.
- **Halves won't pair with each other or the host** — bond table is full or corrupted.

To reset:
1. Flash `nice_nano_v2_settings_reset.uf2` to the **right** half.
2. Flash `eyelash_corne_right` firmware to the **right** half.
3. Flash `nice_nano_v2_settings_reset.uf2` to the **left** half.
4. Flash `eyelash_corne_studio_left` firmware to the **left** half.
5. Power cycle both halves (toggle power switch off, wait 5 seconds, toggle on). They will re-pair automatically.

> **Why right first?** The left half is the BLE central — it initiates connections. If the left half boots with fresh bonds before the right half is reset, it may not discover the right half. Resetting and flashing the right (peripheral) first ensures it's ready to be found when the left half comes up.

### Troubleshooting

**Right half powered on but no keypresses registered:**
The right half connects to the left half over BLE, not directly to the host. A lit display or RGB only confirms power — not a working BLE link. Try these steps in order:

1. **Power cycle both halves** — toggle both power switches off, wait 5 seconds, toggle both on simultaneously.
2. **Check the left half's display** — if it doesn't show a peripheral connection indicator, the halves aren't bonded.
3. **Move the halves close together** — keep them touching during initial pairing; some boards have weak BLE signal on first boot.
4. **Verify correct firmware** — confirm you flashed `eyelash_corne_right` (not left) to the right half.
5. **Full settings reset** — follow the reset procedure above (right half first, then left).

**Keys produce wrong output after a keymap change:**
ZMK Studio overrides persist in flash and take priority over the compiled keymap. Connect the left half via USB-C, open [ZMK Studio](https://zmk.studio/), and check for stale overrides — or do a full settings reset.

**Keyboard won't connect to host computer via Bluetooth:**
Press `BT_CLR_ALL` (Layer 1, leftmost key on the home row) to clear all host Bluetooth bonds, then re-pair from your computer's Bluetooth settings.

## Notes
**This keyboard is not the same as [foostan's Corne](https://github.com/foostan/crkbd). It will not work with standard `corne` firmware.**

If you need a 3D model of this keyboard, email `380465425@qq.com`.

### Soft off
When you press the keys Q, S and Z simultaneously and hold them for 2 seconds, the keyboard will enter a deep sleep state and cannot be awakened by pressing the keys. This function can be used when carrying it outside.  
The activation method is to press the reset switch once. 

This fork was audited for supply-chain and hardware security risks. Below is a summary of findings and actions taken.

### Resolved: Remote board module pinned to local only

The upstream `config/west.yml` pulls the board definition from `github.com/a741725193/zmk-new_corne` at `revision: main` (unpinned). Since this feeds directly into firmware compilation, an unreviewed upstream push could alter pin mappings, flash partitions, or BLE configuration.

**Action taken**: The remote module reference in `config/west.yml` has been commented out. The build now uses only the local `boards/arm/eyelash_corne/` directory. To re-enable the remote, pin it to a specific audited commit SHA.

### Accepted: Keymap Drawer workflow unpinned (`@main`)

The `.github/workflows/draw.yml` workflow references `caksoylar/keymap-drawer` at `@main` with `contents: write` permission. This is a diagram generation utility maintained by a well-known ZMK community contributor. It only produces SVG files in `keymap-drawer/` and cannot affect firmware builds. Commits it creates are clearly labeled `[Draw]`. The risk is negligible for this use case.

### Accepted: ZMK Studio locking disabled

The left half build includes `-DCONFIG_ZMK_STUDIO_LOCKING=n`, allowing keymap changes via USB without authentication. This requires physical access to the keyboard and is appropriate for personal use. Changes made via Studio can be reset. Re-enable locking (`-DCONFIG_ZMK_STUDIO_LOCKING=y`) if the keyboard will be used in a shared or untrusted environment.

## Keymap Diagram

![Diagram of config/eyelash_corne.keymap](keymap-drawer/eyelash_corne.svg) 

_generated by @caksoylar's Keymap Drawer_

