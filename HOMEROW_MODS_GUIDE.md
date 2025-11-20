# Homerow Mods Guide

## Your Homerow Mod Layout

**Left hand (home row)**:
- `A` → tap: a | hold: Cmd/GUI
- `S` → tap: s | hold: Alt
- `D` → tap: d | hold: Shift
- `F` → tap: f | hold: Ctrl

**Right hand (home row)**:
- `J` → tap: j | hold: Ctrl
- `K` → tap: k | hold: Shift
- `L` → tap: l | hold: Alt
- `;` → tap: ; | hold: Cmd/GUI

## How to Use Them

### Basic Concept
- **Quick tap** = normal letter (a, s, d, f, j, k, l, ;)
- **Hold (280ms+)** = modifier key activates

### Common Examples for Sheets:

**Copy/Paste:**
- `Hold F + tap C` = Ctrl+C (copy)
- `Hold F + tap V` = Ctrl+V (paste)
- `Hold F + tap X` = Ctrl+X (cut)

**Navigation:**
- `Hold F + tap Arrow` = Ctrl+Arrow (jump cells)
- `Hold D + click` = Shift+click (select range)
- `Hold D + tap Arrow` = Shift+Arrow (extend selection)

**Undo/Redo:**
- `Hold F + tap Z` = Ctrl+Z (undo)
- `Hold F + Hold D + tap Z` = Ctrl+Shift+Z (redo)

### For Neovim:

**Visual mode selections:**
- `Hold D + tap J/K` = Shift+J/K (visual line mode navigation)

**Window navigation:**
- `Hold J + tap W` = Ctrl+W (window command prefix)

**Normal commands still work:**
- Just tap normally for hjkl navigation, d for delete, etc.

## Smart Features Configured

1. **Cross-hand activation** (`hold-trigger-key-positions`):
   - Left hand mods only activate when you press a key on the RIGHT side
   - Right hand mods only activate when you press a key on the LEFT side
   - This prevents accidental activation when typing same-hand combinations like "sad" or "flask"

2. **Quick-tap (175ms)**:
   - If you're typing fast and repeat a homerow key, it won't activate the modifier
   - Example: "book" won't accidentally trigger mods on the second 'o'

3. **Prior-idle (150ms)**:
   - Requires a brief pause before the mod can activate
   - Prevents accidental mods during fast typing in sheets/Neovim

4. **Balanced flavor**:
   - You can decide tap vs hold even after pressing
   - Good middle ground for sheets and Neovim

## Layer Interaction

### How Homerow Mods Work with Layers:

**On Default Layer:**
- Homerow mods are active on A, S, D, F, J, K, L, ;

**On Lower/Raise Layers:**
- Homerow mods are ALSO active (they follow you to other layers)
- This is because the behavior is defined globally, not per-layer

### Cross-Layer Usage Examples:

**Example 1: Cmd+Arrow Navigation**
```
1. Hold A (activates Cmd)
2. Press and hold thumb for Lower layer (arrows appear)
3. Tap arrow keys
4. Result: Cmd+Arrow navigation works perfectly!
```

**Example 2: Ctrl+Shift+Arrow (select to end of line in sheets)**
```
1. Hold F (activates Ctrl)
2. Hold D (activates Shift)
3. Press and hold thumb for Lower layer
4. Tap Right arrow
5. Result: Ctrl+Shift+Right works!
```

**Example 3: Using homerow mods ON the lower layer**
```
1. Press and hold thumb for Lower layer
2. Hold J (still Ctrl even on lower layer)
3. Tap other keys
4. Result: Ctrl modifier works on lower layer too!
```

### Important Notes:

- **Momentary layer switches preserve modifiers**: When you hold a layer key, any modifiers you're already holding stay active
- **Homerow positions on other layers**: If you have different keys mapped to the A/S/D/F/J/K/L/; positions on other layers, those keys will ALSO have the homerow mod behavior
- **This is powerful for sheets**: You can hold Shift, switch to lower layer, and use arrows for selections seamlessly

## Tips for Learning

1. **Start slow**: Deliberately hold the keys for a full second at first
2. **Practice these combos**:
   - `F+C`, `F+V` (copy/paste)
   - `F+S` (save in sheets/Neovim)
   - `D+Arrow keys` (selections)

3. **If you accidentally trigger a mod**: Just tap the key again quickly

4. **First week expectation**: You'll have some misfires - this is normal!

5. **Rolling vs holding**: You can "roll" across the keys (press mod, then press target while still holding mod)

## Adjustments Available

If after a week you find:
- **Too many accidental triggers**: Increase `tapping-term-ms` to 300-320
- **Too hard to activate**: Decrease to 250-260
- **Still triggering during fast typing**: Increase `require-prior-idle-ms` to 175-200

The settings in `config/corne.keymap` lines 17-19 and 29-31 control this behavior:
```
tapping-term-ms = <280>;        // How long to hold for modifier
quick-tap-ms = <175>;           // Prevent repeated key from activating mod
require-prior-idle-ms = <150>;  // Require pause before mod can activate
```

## Advanced: What's Happening in Other Layers

Currently, your lower and raise layers don't explicitly define homerow mods on their home row positions. This means:

- **If a layer has a transparent binding (`&trans`)** on a homerow position, it falls through to the default layer's homerow mod
- **If a layer has a different key** on a homerow position, that key will have the homerow mod behavior applied to it

For example, on your lower layer position for 'A', you have BT1 (Bluetooth select 1). This means:
- Tap = BT1
- Hold = Cmd (because of the homerow mod behavior)

This might not be desired! You may want to add explicit homerow mods to the lower/raise layers or use `&kp` instead of `&trans` to override the behavior.
