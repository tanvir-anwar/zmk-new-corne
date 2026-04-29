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
| Sh/Caps | Tap-dance — tap for Shift, double-tap for Caps Lock |
| L1, L2, L3 | Momentary layer activate (hold only) |
| L3/Space | Hold for Layer 3, tap for Space |

## Layer 0: QWERTY (default)

| L       | L1 | L2 | L3 | L4       | L5 | R5       | R4 | R3   | R2 | R1 | R    |
|---------|----|----|----|---------|----|---------|----|----- |----|----|----- |
| Tab     | Q  | W  | E  | R       | T  | Y       | U  | I    | O  | P  | Bksp |
| Sh/Caps | A  | S  | D  | F       | G  | H       | J  | K    | L  | ;  | '    |
| Ctrl    | Z  | X  | C  | V       | B  | N       | M  | ,    | .  | /  | Esc  |
|         |    | ⌘  | L1 | L3/Space |    | L3/Enter | L2 | RAlt |    |    |      |

Encoder: Volume Up / Down

## Layer 1: NUMBER (hold L1)

| L   | L1         | L2   | L3   | L4   | L5   | R5   | R4      | R3     | R2      | R1   | R    |
|-----|------------|------|------|------|------|------|---------|--------|---------|------|------|
|     | 1          | 2    | 3    | 4    | 5    | 6    | 7       | 8      | 9       | 0    | Bksp |
|     | BT Clr All | BT 0 | BT 1 | BT 2 | BT 3 | ←    | ↓       | ↑      | →       | Home | PgUp |
|     | RGB Off    | RGB On |    |      | RGB Eff | RGB Efr | RGB Spd | RGB Bri | RGB Brd | End | PgDn |
|     |            |      |      |      |      | Ins  | Del     |        |         |      |      |

Encoder: Scroll Down / Up

## Layer 2: SYMBOL (hold L2)

| L   | L1      | L2     | L3       | L4       | L5  | R5  | R4 | R3 | R2 | R1  | R    |
|-----|---------|--------|----------|----------|-----|-----|----|----|----|-----|------|
|     | !       | @      | #        | $        | %   | ^   | &  | *  | (  | )   | Bksp |
|     | BT Clr  | LClick | MClick   | RClick   | MB4 | -   | =  | [  | ]  | \   | `    |
|     | OUT USB | OUT BLE | _(none)_ | _(none)_ | MB5 | _   | +  | {  | }  | \|  | ~    |
|     |         |        |          | Space    |     | Enter |  |    |    |     |      |

Encoder: Scroll Down / Up

## Layer 3: Fn (hold L3/Space or L3/Enter)

| L             | L1    | L2     | L3         | L4     | L5  | R5         | R4     | R3         | R2     | R1    | R     |
|---------------|-------|--------|------------|--------|-----|------------|--------|------------|--------|-------|-------|
| Studio Unlock | F1    | F2     | F3         | F4     | F5  | F6         | F7     | F8         | F9     | F10   | F11   |
|               |       | LClick | MClick     | RClick | MB4 | Bootloader | LClick | MClick     | RClick | PrtSc | F12   |
|               | Reset |        | Bootloader |        | MB5 |            |        | Bootloader | Reset  | ScrLk | Pause |
|               |       |        |            |        |     |            |        |            |        |       |       |

Encoder: RGB Brightness Up / Down

## Combos

| Keys                  | Action                  |
|-----------------------|-------------------------|
| Q + S + Z (hold 2s)  | Soft off (deep sleep)   |
