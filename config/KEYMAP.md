# Eyelash Corne Keymap Reference

This is a human-readable reference of the keymap defined in `eyelash_corne.keymap`.
Edit this file to describe desired changes, then update the keymap to match.

The extra key (left of 5-way switch) and 5-way switch are omitted for clarity.

## Legend

| Notation | Meaning |
|----------|---------|
| _(blank)_ | Transparent — falls through to the layer below |
| _(none)_ | No action |
| hold/tap | Hold for first action, tap for second |
| [Sh/Caps] | Tap-dance — tap for Shift, double-tap for Caps Word |
| L1/Space | Hold for Layer 1 (Symbol), tap for Space |
| L2/Enter | Hold for Layer 2 (Function), tap for Enter |
| <K1/K2> | Mod morph — K1 for normal key press, K2 when RCTRL is pressed |
| ⌘+key | Modified keycode — sends Cmd+key (not a macro) |
| ⌘⇧4 | Screenshot macro — sends Cmd+Shift+4 |

### ZMK Reference
1. Keycodes: https://zmk.dev/docs/keymaps/list-of-keycodes
2. Mod Morph: https://zmk.dev/docs/keymaps/behaviors/mod-morph
3. Hold Tap: https://zmk.dev/docs/keymaps/behaviors/hold-tap

## Layer 0: QWERTY (default)

| L       | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R             |
|---------|----|----|----|----|----|----|----|----|----|----|---------------|
| <TAB/GRAVE> | Q  | W  | E  | R  | T  | Y  | U  | I  | O  | P  | <MINUS/EQUAL> |
| BSPC    | A  | S  | D  | F  | G  | H  | J  | K  | L  | ;  | '             |
| [Sh/Caps] | Z  | X  | C  | V  | B  | N  | M  | ,  | .  | /  | RSHIFT        |
|         |    | ESC/LALT | LCMD | L1/Space | | Space | L2/Enter | RCTRL | | | |

Encoder: Volume Up / Down
Arrow Keys: Arrow Keys

## Layer 1: SYMBOL (hold L1)

| L      | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1   | R      |
|--------|----|----|----|----|----|----|----|----|----|----- |--------|
| GRAVE  | !  | @  | #  | $  | %  | ^  | &  | *  | (  | )    | +  |
| BSPC   | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  | 0    | MINUS  |
| LSHIFT | BSLH  | PIPE | _ | ~  | =  | [  | ]  | {  | }  | / | RSHIFT |
|        |    | LALT | LCMD |  |    | Space | Enter | RCTRL | | | |

Encoder: Scroll Down / Up
Arrow Keys: Arrow Keys

## Layer 2: FUNCTION (hold L2)

| L    | L1         | L2       | L3         | L4    | L5       | R5       | R4       | R3       | R2       | R1     | R          |
|------|------------|----------|------------|-------|----------|----------|----------|----------|----------|--------|------------|
|      | F3         | Mute     | ⌘⇧4       |       |          |          |          |          |          |        |            |
|      | BT Clr All | BT 0     | BT 1       | BT 2  | USB      | ←        | ↓        | ↑        | →        | LClick | RClick     |
|      | RGB Off    | RGB On   | RGB Eff    | Reset | Soft Off | ⌘+←      | ⌘+↓     | ⌘+↑     | ⌘+→     |        | Bootloader |
|      |            |          |            |       |          |          |          |          |          |        |            |

Encoder: Brightness Up / Down
Arrow Keys: Arrow Keys

## Combos

| Keys                 | Action                |
|----------------------|-----------------------|
| Q + S + Z (hold 2s)  | Soft off (deep sleep) |
