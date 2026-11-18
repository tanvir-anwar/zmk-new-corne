# Eyelash Corne Keymap Reference

This is a human-readable reference of the keymap defined in `eyelash_corne.keymap`.
Edit this file to describe desired changes, then update the keymap to match.

The extra key (left of 5-way switch) and 5-way switch are omitted for clarity.

## Design Philosophy

- **Traditional grouping:** numbers and their shifted symbols retain standard ordering,
  while related punctuation stays grouped on the Symbol layer. Opening and closing
  brackets are stacked vertically (`[ ]`, `{ }`, `( )`).
- **Code-first Symbol layer:** brackets, braces, parentheses, and common code
  punctuation are direct Symbol-layer keys rather than shifted variants, so snippets
  need no modifier juggling. The Symbol layer carries no modifiers.
- **At most two simultaneous keys for every symbol:** symbols use either a direct key,
  a layer chord, or sticky Shift followed by a layer chord (for example, `!@#$%^&*`).
  Sticky Shift avoids three-finger chords while preserving familiar shifted-number
  relationships.
- Thumb cluster: the left thumbs provide `Cmd/Tab`, Backspace, and sticky Shift.
  `L2/Space` and `L3/Enter` use tap-preferred layer-taps, while the right outer
  thumb holds `Ctrl` or taps `Esc`.
- Pinky relief: high-frequency editing and primary modifiers live on the thumbs.
  `Alt` remains available from the left outer column, from `ALT/Z`, and as a sticky
  modifier on Function; the right pinky retains `SHIFT/FSLH`.
- Identifier-friendly punctuation: `Semicolon` is on the Colemak base layer, `Minus`
  and `Underscore` are direct keys on the Symbol layer, and Caps Word continues
  through `Minus`.
- Avoid tap dance and printable-key combos for routine character entry. Reserve
  combos for infrequent system or layout actions, and generally avoid timing
  ambiguity on alpha keys. `ALT/Z` is the intentional exception because Z is the
  least frequent letter in English and keeps Alt available in the 36-key layout.
- Sticky `Shift` can be tapped before Symbol or while Symbol is held, then applies to
  the next number or punctuation key.
- Modifier shortcuts that need a number or symbol (for example, `Cmd+1`) use a sticky
  modifier from the Function layer, followed by the Symbol-layer chord.
- Thumb bindings stay transparent on higher layers so their behavior remains consistent.
- Colemak is a gradual transition toward a 36-key workflow: preserve familiar
  number ordering and mnemonic controls while keeping symbols and navigation accessible.

## Legend

| Notation | Meaning |
|----------|---------|
| _(blank)_ | Transparent — falls through to the layer below |
| _(none)_ | No action |
| hold/tap | Hold for first action, tap for second |
| Ln/key | Tap-preferred layer-tap — hold for Layer n, tap for the key |
| &sk MOD | Sticky modifier — tap for one-shot use, or hold as a normal modifier |
| &tog Ln | Toggle Layer n on/off |
| ⌘+key | Modified keycode — sends Cmd+key (not a macro) |
| ⌘⇧4 | Screenshot macro — sends Cmd+Shift+4 |

### ZMK Reference
1. Keycodes: https://zmk.dev/docs/keymaps/list-of-keycodes
2. Hold Tap: https://zmk.dev/docs/keymaps/behaviors/hold-tap
3. Sticky Key: https://zmk.dev/docs/keymaps/behaviors/sticky-key

## Layer 0: Colemak-DH (default)

| L     | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R       |
|-------|----|----|----|----|----|----|----|----|----|----|---------|
| GRAVE | Q  | W  | F  | P  | B  | J  | L  | U  | Y  | ;  | MINUS   |
| Caps Lock | A  | R  | S  | T  | G  | M  | N  | E  | I  | O  | '  |
| ALT | ALT/Z  | X  | C  | D  | V  | K  | H  | ,  | .  | SHIFT/FSLH | SHIFT |
|  |  |  | CMD/TAB | BSPC | &sk SHIFT | L2/Space | L3/Enter | CTRL/ESC | | |  |

Encoder: Volume Up / Down
Joystick: Arrow Keys

## Layer 1: QWERTY

From Colemak-DH, press the sticky Shift thumb and the `L2/Space` thumb together
to switch to QWERTY. Press the same physical chord again to return to Colemak-DH.

The two outer columns remain transparent and fall back to the matching keys on
the Colemak-DH layer.

| L | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R  |
|---|----|----|----|----|----|----|----|----|----|----|----|
|   | Q  | W  | E  | R  | T  | Y  | U  | I  | O  | P  |    |
|   | A  | S  | D  | F  | G  | H  | J  | K  | L  | ;  |    |
|   | ALT/Z  | X  | C  | V  | B  | N  | M  | ,  | . | SHIFT/FSLH |  |
|   |    |    |    |    |    |    |    |    |   |     |    |

Encoder: Volume Up / Down
Joystick: Arrow Keys

## Layer 2: SYMBOL (hold L2/Space)

| L |   L1  | L2| L3| L4 | L5 |  R5  | R4| R3| R2 | R1 | R |
|---|-------|---|---|----|----|------|---|---|----|----|---|
|   |   1   | 2 | 3 | 4  | 5  |  6   | 7 | 8 | 9  | 0  |   |
|   | GRAVE | ~ | [ |  { | (  | PIPE | - | = | "  | '  |   |
|   |       |   | ] |  } | )  | BSLH | _ |   |    |    |   |
|   |       |   |   |    |    |      |   |   |    |    |   |

Encoder: Scroll Down / Up
Joystick: Arrow Keys

## Layer 3: FUNCTION (hold L3)

Hold `L3/Enter` and press the physical T position (`&tog L2`) to lock or unlock
the Symbol layer.

| L      | L1        | L2       | L3       | L4     | L5        | R5 | R4     | R3    | R2  | R1    | R          |
|--------|-----------|----------|---------|---------|-----------|----|--------|-------|-----|-------|------------|
| Reset  |    TAB    | Mute     |   F3    | ⌘⇧4     | &tog L2   |    | LClick | RClick|     | MINUS | Bootloader |
| BT Clr All | Caps Lock | &sk CTRL | &sk ALT | &sk CMD | Caps Word | ←  | ↓      | ↑     | →   | '     |   |
| Soft Off | RGB Off | RGB On   | BT 0    | BT 1    | BT 2      | PgDn | PgUp |       |     |       |   |
|        |           |          |         |         |           |      |      |       |     |       |   |

Encoder: Brightness Up / Down
Joystick: Mouse cursor (center-press = Left Click)

## Combos

| Keys                                  | Action                |
|---------------------------------------|-----------------------|
| Q + S + Z (hold 2s)                   | Soft off (deep sleep) |
| Sticky Shift thumb + L2/Space thumb   | QWERTY ↔ Colemak-DH   |
