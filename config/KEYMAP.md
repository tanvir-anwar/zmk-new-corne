# Eyelash Corne Keymap Reference

This is a human-readable reference of the keymap defined in `eyelash_corne.keymap`.
Edit this file to describe desired changes, then update the keymap to match.

The extra key (left of 5-way switch) and 5-way switch are omitted for clarity.

## Design Philosophy

- **Traditional placement:** where possible, a key sits as close to its position on a
  traditional QWERTY keyboard as the layout allows (e.g. `` ` ``/`~` in the top-left
  corner, brackets on the right). This keeps existing muscle memory intact.
- **At most two simultaneous keys for every symbol:** symbols use either a direct key,
  a layer chord, or a sticky modifier followed by a layer chord. Sticky modifiers avoid
  three-finger chords while preserving familiar shifted-number relationships.
- Thumb-cluster mods: sticky `Cmd` and `Shift` live on the left thumbs. The right outer
  thumb holds `Ctrl` or taps `Minus`, keeping both actions available without loading an
  alpha key. `Bksp`, `Space`, and `L4/Enter` retain dedicated thumb positions.
- Pinky relief: high-frequency editing and primary modifiers live on the thumbs.
  `Alt` remains available from `ALT/,` and as a sticky modifier on Layer 4; the right
  pinky retains `SHIFT/FSLH`.
- Identifier-friendly punctuation: `Semicolon` is on the Colemak base layer, `Minus`
  is on the right thumb, and Caps Word continues through `Minus`.
- Avoid tap dance and printable-key combos for routine character entry. Reserve
  combos for infrequent system or layout actions, and do not put timing ambiguity
  on plain alpha keys.
- `GLOBE` works for Globe-chords (emoji, Mission Control), not raw `Fn+key`.
- Sticky `Shift` can be tapped before Layer 4 or while Layer 4 is held, then applies to
  the next number, bracket, or punctuation key.
- L1 thumb on Layer 1 must stay transparent (it unlocks the toggled Symbol layer).
- Colemak-DH is a gradual transition toward a 36-key workflow: preserve familiar
  number ordering and mnemonic controls while keeping symbols and navigation accessible.

## Legend

| Notation | Meaning |
|----------|---------|
| _(blank)_ | Transparent — falls through to the layer below |
| _(none)_ | No action |
| hold/tap | Hold for first action, tap for second |
| &mo_tog Ln | Hold-tap — hold for momentary Layer n, tap to toggle Layer n on/off (sticky) |
| &sk MOD | Sticky modifier — tap for one-shot use, or hold as a normal modifier |
| [Ctrl→L4] | Mod-morph — while Ctrl is active, toggle Layer 4 instead of the normal key action |
| CTRL/MINUS | Hold for Ctrl, tap for Minus |
| FUNC/BSLH | Hold for macOS Fn/Globe key, tap for Backslash |
| L2/Enter | Hold for Layer 2 (Function), tap for Enter |
| L4/Enter | Hold for Layer 4 (Symbol-DH), tap for Enter |
| ⌘+key | Modified keycode — sends Cmd+key (not a macro) |
| ⌘⇧4 | Screenshot macro — sends Cmd+Shift+4 |
| ⌘+Click | Macro — holds Cmd while left-clicking (open link in new tab, multi-select) |

### ZMK Reference
1. Keycodes: https://zmk.dev/docs/keymaps/list-of-keycodes
2. Mod Morph: https://zmk.dev/docs/keymaps/behaviors/mod-morph
3. Hold Tap: https://zmk.dev/docs/keymaps/behaviors/hold-tap
4. Sticky Key: https://zmk.dev/docs/keymaps/behaviors/sticky-key

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
|        | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  | 0  |    |
|        |    |    |    |    |    | [  | ]  |    |    |    |    |
|        |    |    |    |    |    |    |    |    |    |    |    |

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
| ~ | TAB        | Mute   |   F3    | ⌘⇧4   |          |     |     |     |     |        | Bootloader |
| BT Clr All | ESC | BT 0 | BT 1    | BT 2  | USB      | ←   | ↓   | ↑   | →   | LClick | RClick     |
|   | RGB Off    | RGB On | RGB Eff | Reset | Soft Off | ⌘+← | PgDn | PgUp | ⌘+→ | ⌘+Click |         |
|   |            |        |         |       |          |     |     |     |     |        |            |

Encoder: Brightness Up / Down
Joystick: Mouse cursor (center-press = Left Click)

## Layer 3: Colemak-DH

From QWERTY, press the physical left outer thumb key and the Space key
together to switch to Colemak-DH. Press the same physical chord again to
return to QWERTY.

The two outer columns remain transparent as temporary training aids while
transitioning toward a 36-key layout.

Hold the `CTRL/MINUS` thumb and tap the comma key to toggle Layer 4. The
same chord toggles Layer 4 off.

| L        | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R  |
|----------|----|----|----|----|----|----|----|----|----|----|----|
|          | Q  | W  | F  | P  | B  | J  | L  | U  | Y  | ;  |    |
|          | A  | R  | S  | T  | G  | M  | N  | E  | I  | O  |    |
|          | Z  | X  | C  | D  | V  | K  | H  | ALT/, [Ctrl→L4] | . | SHIFT/FSLH |  |
| | | | BSPC | &sk CMD | &sk SHIFT | Space | L4/Enter | CTRL/MINUS | | | |

## Layer 4: Symbol-DH

From Colemak-DH, hold the right `L4/Enter` thumb for momentary access. Hold the
`CTRL/MINUS` thumb and tap the comma key to toggle Symbol-DH on or off.

Sticky `Shift` remains active through the Layer 4 binding. Tap sticky `Shift`,
then hold `L4/Enter` and press a number or bracket to produce its shifted symbol
without a three-finger chord. Alternatively, hold `L4/Enter`, tap the Layer 4
sticky `Shift`, then press the symbol key.

Caps Word remains active through `Minus`, allowing identifiers such as
`DO-NOT-DELETE` without reactivating Caps Word.

As on Colemak-DH, the two outer columns remain transparent during the
transition to 36 keys.

| L | L1 | L2 | L3 | L4 | L5 | R5 | R4 | R3 | R2 | R1 | R  |
|---|----|----|----|----|----|----|----|----|----|----|----|
|   | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  | 0  |    |
|   | `  | BSLH |  [ | ] | GLOBE | ←  | ↓  | ↑  | →  | ' |    |
|   | TAB | ⌘⇧4 | Caps Word | &sk ALT | ESC |  PgDn |  PgUp | , [Ctrl→L4] | EQUAL | SHIFT |   |
|   |    |    | BSPC | &sk CMD | &sk SHIFT | Space | L4/Enter | CTRL/MINUS |   |   |   |

## Combos

| Keys                                  | Action                |
|---------------------------------------|-----------------------|
| Q + S + Z (hold 2s)                   | Soft off (deep sleep) |
| Physical left outer thumb + Space     | QWERTY ↔ Colemak-DH   |
