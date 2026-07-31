# Eyelash Corne Keymap Reference

This is a human-readable reference of the keymap defined in `eyelash_corne.keymap`.
Edit this file to describe desired changes, then update the keymap to match.

The extra key (left of 5-way switch) and 5-way switch are omitted for clarity.

## Design Philosophy

- **Traditional placement:** where possible, a key sits as close to its position on a
  traditional QWERTY keyboard as the layout allows (e.g. `` ` ``/`~` in the top-left
  corner, brackets on the right). This keeps existing muscle memory intact.
- **At most a 2-key chord for every symbol:** every symbol is reachable in ≤2 keys —
  a single base-layer tap, or one modifier held with one key (`Shift`, `L1`/Symbol, or
  `L2`/Function). No 3-key symbol chords. When a symbol leaves its dedicated key it
  must still satisfy this (e.g. `~` = `Shift`+`` ` ``, `_` = `Shift`+`-`).
- Thumb-cluster mods (restore prior muscle memory): `Ctrl` on the **right thumb**
  (hold Ctrl / tap Esc) for unix/terminal chords; `Cmd` on the **left thumb** (Mac
  position); `Bksp` on the **left-thumb outer** — the logical reverse of Enter, on the
  opposite hand, so a mishit lands on `Cmd` (harmless) rather than `Enter`.
- Pinky relief: high-frequency `Bksp` and the mods live on the thumbs, not the weak
  pinkies. `Alt` drops to the **left-of-Z pinky** as `Alt/=` (keeps the `=` tap the
  Corne has no number row for); rare `FUNC/BSLH` sits on the **left-of-A pinky**.
- Repurpose redundant keys: left Space → symbol-layer lock. Right pinky is a plain
  cross-hand `Shift`; `Caps_Word` moves to the **Symbol layer** (same pinky key) to
  avoid the base-layer hold-tap misfiring Caps Word during shift-rolls.
- Prefer hold-taps over tap-dance/mod-morph; guard pinky holds with `require-prior-idle-ms`.
- `GLOBE` works for Globe-chords (emoji, Mission Control), not raw `Fn+key`.
- Single Shift (right pinky, plain `&kp`) — revisit if same-hand shifting feels awkward.
- L1 thumb on Layer 1 must stay transparent (it unlocks the toggled Symbol layer).

## Legend

| Notation | Meaning |
|----------|---------|
| _(blank)_ | Transparent — falls through to the layer below |
| _(none)_ | No action |
| hold/tap | Hold for first action, tap for second |
| SHIFT/Caps_Word | Hold-tap — hold for Shift, tap for Caps Word |
| &mo_tog L1 | Hold-tap — hold for momentary Layer 1, tap to toggle Layer 1 on/off (sticky) |
| FUNC/BSLH | Hold for macOS Fn/Globe key, tap for Backslash |
| L2/Enter | Hold for Layer 2 (Function), tap for Enter |
| ⌘+key | Modified keycode — sends Cmd+key (not a macro) |
| ⌘⇧4 | Screenshot macro — sends Cmd+Shift+4 |
| ⌘+Click | Macro — holds Cmd while left-clicking (open link in new tab, multi-select) |

### ZMK Reference
1. Keycodes: https://zmk.dev/docs/keymaps/list-of-keycodes
2. Mod Morph: https://zmk.dev/docs/keymaps/behaviors/mod-morph
3. Hold Tap: https://zmk.dev/docs/keymaps/behaviors/hold-tap

## Layer 0: QWERTY (default)

| L        | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R       |
|----------|----|----|----|----|----|----|----|----|----|----|---------|
| TAB      | Q  | W  | E  | R  | T  | Y  | U  | I  | O  | P  | MINUS   |
| FUNC/BSLH | A  | S  | D  | F  | G  | H  | J  | K  | L  | ;  | '       |
| ALT/EQUAL | Z  | X  | C  | V  | B  | N  | M  | ,  | .  | /  | SHIFT   |
| | | | BSPC | LCMD | &mo_tog L1 | Space | L2/Enter | CTRL/ESC | | |    |

Encoder: Volume Up / Down
Arrow Keys: Arrow Keys

## Layer 1: SYMBOL (hold L1)

| L      | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R  |
|--------|----|----|----|----|----|----|----|----|----|----|----|
| GRAVE  | !  | @  | #  | $  | %  | ^  | &  | *  | (  | )  |    |
|        | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  | 0       |
|        |    |    |    |    |    | [  | ]  | {  | }  | /  | Caps_Word |
|    |  |    |    | LCMD+SHIFT |  |    |    |    |    |    |   |

Encoder: Scroll Down / Up
Arrow Keys: Arrow Keys

> ⚠️ The L1 thumb position (under `&mo_tog 1 1` on Layer 0) is intentionally
> left **transparent** here. Tapping `mo_tog` locks Layer 1 on; the unlock tap
> falls through this transparent key to Layer 0's `mo_tog` to toggle it back off.
> Do **not** assign a real binding here, or you will lose the ability to exit
> the locked Symbol layer.

## Layer 2: FUNCTION (hold L2)

| L | L1         | L2     | L       | L4    | L5       | R5  | R4  | R3  | R2  | R1     | R          |
|---|------------|--------|---------|-------|----------|-----|-----|-----|-----|--------|------------|
| ~ |            | Mute   |   F3    | ⌘⇧4   |          |     |     |     |     |        | Bootloader |
| BT Clr All | BT 0 | BT 1 |        |       | USB      | ←   | ↓   | ↑   | →   | LClick | RClick     |
|   | RGB Off    | RGB On | RGB Eff | Reset | Soft Off | ⌘+← | PgDn | PgUp | ⌘+→ | ⌘+Click |         |
|   |            |        |         |       |          |     |     |     |     |        |            |

Encoder: Brightness Up / Down
Joystick: Mouse cursor (center-press = Left Click)

## Combos

| Keys                 | Action                |
|----------------------|-----------------------|
| Q + S + Z (hold 2s)  | Soft off (deep sleep) |
