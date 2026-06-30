# Eyelash Corne Keymap Reference

This is a human-readable reference of the keymap defined in `eyelash_corne.keymap`.
Edit this file to describe desired changes, then update the keymap to match.

The extra key (left of 5-way switch) and 5-way switch are omitted for clarity.

## Design Philosophy

- Modifier order Ctrl > Opt > Cmd to match other keyboards (least relearning).
- Pinky-relief rotation: `BSPC` lives on the **left thumb** (not the weak pinky);
  `Cmd` on the **right thumb** (also makes ⌘+C/V/Z/X cross-hand); rare `FUNC/BSLH`
  drops to the **left-of-A pinky**. (Cmd on the right hand intentionally relaxes the
  strict left-hand Ctrl>Opt>Cmd ordering for the high-frequency Backspace.)
- Repurpose redundant keys: left Space → symbol-layer lock; LSHIFT → `caps` pinky.
- Prefer hold-taps over tap-dance/mod-morph; guard pinky holds with `require-prior-idle-ms`.
- `GLOBE` works for Globe-chords (emoji, Mission Control), not raw `Fn+key`.
- Single Shift (right pinky) — revisit if same-hand shifting feels awkward.
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

### ZMK Reference
1. Keycodes: https://zmk.dev/docs/keymaps/list-of-keycodes
2. Mod Morph: https://zmk.dev/docs/keymaps/behaviors/mod-morph
3. Hold Tap: https://zmk.dev/docs/keymaps/behaviors/hold-tap

## Layer 0: QWERTY (default)

| L        | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R       |
|----------|----|----|----|----|----|----|----|----|----|----|---------|
| TAB      | Q  | W  | E  | R  | T  | Y  | U  | I  | O  | P  | MINUS   |
| FUNC/BSLH | A  | S  | D  | F  | G  | H  | J  | K  | L  | ;  | '       |
| CTRL/ESC | Z  | X  | C  | V  | B  | N  | M  | ,  | .  | /  | SHIFT/Caps_Word |
| | | | LALT/EQUAL | BSPC | &mo_tog L1 | Space | L2/Enter | LCMD | | |    |

Encoder: Volume Up / Down
Arrow Keys: Arrow Keys

## Layer 1: SYMBOL (hold L1)

| L      | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1  | R |
|--------|----|----|----|----|----|----|----|----|----|-----|---|
| ~      | !  | @  | #  | $  | %  | ^  | &  | *  | (  | )   |   |
| GRAVE  | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  | 0   |   |
|     | BSLH  | PIPE | _ | ~  | =  | [  | ]  | {  | }  | /  |   |
|        |    |      |   |    |    |    |    |    |    |    |   |

Encoder: Scroll Down / Up
Arrow Keys: Arrow Keys

> ⚠️ The L1 thumb position (under `&mo_tog 1 1` on Layer 0) is intentionally
> left **transparent** here. Tapping `mo_tog` locks Layer 1 on; the unlock tap
> falls through this transparent key to Layer 0's `mo_tog` to toggle it back off.
> Do **not** assign a real binding here, or you will lose the ability to exit
> the locked Symbol layer.

## Layer 2: FUNCTION (hold L2)

| L | L1         | L2     | L       | L4    | L5       | R5  | R4  | R3  | R2  | R1     | R      |
|---|------------|--------|---------|-------|----------|-----|-----|-----|-----|--------|--------|
|   |            | Mute   |   F3    | ⌘⇧4   |          |     |     |     |     |        |        |
|   | BT Clr All | BT 0   |         |       | USB      | ←   | ↓   | ↑   | →   | LClick | RClick |
|   | RGB Off    | RGB On | RGB Eff | Reset | Soft Off | ⌘+← | PgDn | PgUp | ⌘+→ |        | Bootloader |
|   |            |        |         |       |          |     |     |     |     |        |            |

Encoder: Brightness Up / Down
Arrow Keys: Arrow Keys

## Combos

| Keys                 | Action                |
|----------------------|-----------------------|
| Q + S + Z (hold 2s)  | Soft off (deep sleep) |
