# Eyelash Corne Keymap Reference

This is a human-readable reference of the keymap defined in `eyelash_corne.keymap`.
Edit this file to describe desired changes, then update the keymap to match.

The extra key (left of 5-way switch) and 5-way switch are omitted for clarity.

## Design Philosophy

- **Traditional grouping:** numbers and their shifted symbols retain standard ordering,
  while related punctuation stays grouped on the Symbol layer.
- **At most two simultaneous keys for every symbol:** symbols use either a direct key,
  a layer chord, or a sticky modifier followed by a layer chord. Sticky modifiers avoid
  three-finger chords while preserving familiar shifted-number relationships.
- Thumb-cluster mods: dedicated `Cmd` and sticky `Shift` live on the left thumbs.
  `L2/Space` and `L3/Enter` use tap-preferred layer-taps, while the right outer
  thumb holds `Ctrl` or taps `Esc`.
- Pinky relief: high-frequency editing and primary modifiers live on the thumbs.
  `Alt` remains available from the left outer column, from `ALT/Z`, and as a sticky
  modifier on Symbol; the right pinky retains `SHIFT/FSLH`.
- Identifier-friendly punctuation: `Semicolon` is on the Colemak base layer, `Minus`
  is on the Symbol layer, and Caps Word continues through `Minus`.
- Avoid tap dance and printable-key combos for routine character entry. Reserve
  combos for infrequent system or layout actions, and generally avoid timing
  ambiguity on alpha keys. `ALT/Z` is the intentional exception because Z is the
  least frequent letter in English and keeps Alt available in the 36-key layout.
- Sticky `Shift` can be tapped before Symbol or while Symbol is held, then applies to
  the next number, bracket, or punctuation key.
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

## Layer 0: QWERTY (default)

| L     | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R       |
|-------|----|----|----|----|----|----|----|----|----|----|---------|
| TAB   | Q  | W  | E  | R  | T  | Y  | U  | I  | O  | P  | MINUS   |
| Caps Lock | A  | S  | D  | F  | G  | H  | J  | K  | L  | ;  | '  |
| ALT | ALT/Z  | X  | C  | V  | B  | N  | M  | ,  | .  | SHIFT/FSLH | SHIFT |
|  |  |  | BSPC | CMD | &sk SHIFT | L2/Space | L3/Enter | CTRL/ESC | | |  |

Encoder: Volume Up / Down
Joystick: Arrow Keys

## Layer 1: Colemak

From QWERTY, press the sticky Shift thumb and the `L2/Space` thumb together to
switch to Colemak (vanilla, NOT DH). Press the same physical chord again to return
to QWERTY.

The two outer columns remain transparent as temporary training aids while
transitioning toward a 36-key layout.

| L | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R  |
|---|----|----|----|----|----|----|----|----|----|----|----|
|   | Q  | W  | F  | P  | G  | J  | L  | U  | Y  | ;  |    |
|   | A  | R  | S  | T  | D  | H  | N  | E  | I  | O  |    |
|   | ALT/Z  | X  | C  | V  | B  | K  | M  | ,  | . | SHIFT/FSLH |  |
|   |    |    |    |    |    |    |    |    |   |     |    |

Encoder: Volume Up / Down
Joystick: Arrow Keys

## Layer 2: SYMBOL (hold L2/Space; lock with &tog L2)

| L | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R  |
|---|----|----|----|----|----|----|----|----|----|----|----|
|   | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  | 0  |    |
|   | GRAVE | BSLH | [ |  ] | Caps Word | &sk SHIFT | &sk CMD | &sk ALT | &sk CTRL | ' |   |
|   |    |    |  | &tog L2 | | EQUAL | MINUS |   |    |    |    |
|   |    |    |    |    |    |    |    |    |    |    |    |

Encoder: Scroll Down / Up
Joystick: Arrow Keys

## Layer 3: FUNCTION (hold L3)

| L      | L1        | L2       | L3       | L4     | L5        | R5 | R4     | R3    | R2  | R1    | R          |
|--------|-----------|----------|---------|---------|-----------|----|--------|-------|-----|-------|------------|
| Reset  |    TAB    | Mute     |   F3    | ⌘⇧4     |           |    | LClick | RClick|     | MINUS | Bootloader |
| BT Clr All | Caps Lock | &sk CTRL | &sk ALT | &sk CMD | Caps Word | ←  | ↓      | ↑     | →   | '     |   |
| Soft Off | RGB Off | RGB On   | BT 0    | BT 1    | BT 2      | PgDn | PgUp |       |     |       |   |
|        |           |          |         |         |           |      |      |       |     |       |   |

Encoder: Brightness Up / Down
Joystick: Mouse cursor (center-press = Left Click)

## Combos

| Keys                                  | Action                |
|---------------------------------------|-----------------------|
| Q + S + Z (hold 2s)                   | Soft off (deep sleep) |
| Sticky Shift thumb + L2/Space thumb   | QWERTY ↔ Colemak      |
