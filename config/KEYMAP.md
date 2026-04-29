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
| L1, L2, L3 | Momentary layer activate (hold only) |
| L3/Space | Hold for Layer 3, tap for Space |
| <K1/K2> | Mod morph - K1 for normal key press, K2 when RCTRL is pressed |  

### ZMK Reference*
1. Keycodes: https://zmk.dev/docs/keymaps/list-of-keycodes
2. Mod Morph: https://zmk.dev/docs/keymaps/behaviors/mod-morph
3. Hold Tap: https://zmk.dev/docs/keymaps/behaviors/hold-tap 

## Layer 0: QWERTY (default)

| L       | L1 | L2 | L3 | L4      | L5 | R5      | R4 | R3   | R2 | R1 | R    |
|---------|----|----|----|---------|----|---------|----|----- |----|----|----- |
| Tab     | Q  | W  | E  | R       | T  | Y       | U  | I    | O  | P  | <MINUS/EQUAL> |
| BSPC    | A  | S  | D  | F       | G  | H       | J  | K    | L  | ;  | '    |
| [Sh/Caps] | Z  | X  | C  | V       | B  | N       | M  | ,    | .  | /  | RSHIFT |
|  |  | GRAVE/LALT | Esc/LCMD | L2/Space |   | L1/Space | L3/Enter | RCTRL |  |  |  |

Encoder: Volume Up / Down
Arrow Keys: Arrow Keys

## Layer 1: NUMBER (hold L1)

| L   | L1         | L2   | L3   | L4   | L5   | R5   | R4      | R3     | R2      | R1   | R    |
|-----|------------|------|------|------|------|------|---------|--------|---------|------|------|
|     | 1          | 2    | 3    | 4    | 5    | 6    | 7       | 8      | 9       | 0    | Bksp |
|     | BT Clr All | BT 0 | BT 1 | BT 2 | BT 3 | ←    | ↓       | ↑      | →       | Home | PgUp |
|     | RGB Off    | RGB On |    |      | RGB Eff | RGB Efr | RGB Spd | RGB Bri | RGB Brd | End | PgDn |
|     |            |      |      |      |      | Ins  | Del     |        |         |      |      |

Encoder: Scroll Down / Up
Arrow Keys: Arrow Keys

## Layer 2: SYMBOL (hold L2)

| L     | L1 | L2 | L3 | L4 | L5  | R5 | R4 | R3 | R2 | R1  | R     |
|-------|----|----|----|----|-----|----|----|----|----|-----|-------|
| GRAVE | !  | @  | #  | $  | %   | ^  | &  | *  | (  | )   | EQUAL |
| BSPC  | 1  | 2  | 3  | 4  | 5   | 6  | 7  | 8  | 9  | 0   | MINUS |
| LSHIFT|    |    |    | +  | /   | [  | ]  | {  | }  | BACKSLASH | RSHIFT |
|     |    | LALT | Esc/LCMD |    |    | Space | Enter | RCTRL |  |  |   |

Encoder: Scroll Down / Up
Arrow Keys: Arrow Keys

## Layer 3: Fn (hold L3/Space or L3/Enter)

| L             | L1    | L2     | L3         | L4     | L5  | R5         | R4     | R3         | R2     | R1    | R     |
|---------------|-------|--------|------------|--------|-----|------------|--------|------------|--------|-------|-------|
| Studio Unlock | F1    | F2     | F3         | F4     | F5  | F6         | F7     | F8         | F9     | F10   | F11   |
|               | BT Clr All  | BT Clr |   |  |   | Bootloader | LClick |     | RClick | PrtSc | F12   |
|               | Reset | Soft off  | Bootloader |        |  |            |        | Bootloader | Reset  | ScrLk | Pause |
|               |       |        |            |        |     |            |        |            |        |       |       |

Encoder: RGB Brightness Up / Down

## Combos

| Keys                  | Action                  |
|-----------------------|-------------------------|
| Q + S + Z (hold 2s)  | Soft off (deep sleep)   |
