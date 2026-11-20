# Notes for Claude

## Setup
- Two Cornes, one shared keymap (`config/corne_shared.dtsi`):
  - Typeractive = classic 42-key (nice!nano v2 + nice!view): `config/corne.keymap`, `config/corne.conf`
  - Alibaba = Eyelash Corne (joystick + knob, unused): `config/eyelash_corne.keymap`, `config/eyelash_corne.conf`;
    board module pinned to commit a7d93082 (newer upstream needs forked ZMK)
- Layout: markstos-based, tuned for Windows + Mac. `main` is the live layout.
  - `markstos-stock` = history of the markstos port (kept, no longer worked on)
  - `original-layout` = the pre-markstos layout
- Mac: System Settings > Keyboard > Modifier Keys swaps Control <-> Command for the Corne,
  so firmware is written Windows-style (Ctrl). Mac-specific bindings must account for the swap.
- User: Windows at home, Mac at work, uses nvim (avoid combos that break nvim, e.g. D+F broke `df`).

## Workflow
- Use jj, not git. Run `jj git fetch` first: the Draw workflow pushes `[Draw]` commits.
- No history-rewriting pushes (blocked). Merge, don't rebase.
- Every push builds firmware in GitHub Actions; the user flashes left + right for each keyboard.
- Always show the exact diff and wait for an explicit "go" before editing or pushing.

## Decisions & lessons
- Thumb hold-taps (Backspace/RAISE, Tab/LOWER, Enter/GUI, Esc/Ctrl) send the tap on release,
  so a quick tap lags slightly. Fixed with `require-prior-idle-ms = 150` (instant tap while typing)
  and `quick-tap-ms = 150` (tap then hold = repeat). Plain keys are the only zero-lag option.
- Typeractive uses `CONFIG_ZMK_KSCAN_DEBOUNCE_PRESS_MS=2` (Eyelash too). Release debounce is
  left at default on Typeractive, since a longer release debounce delays hold-tap taps.
- Keys felt fast on Windows but laggy on the Mac. Suspect Mac Bluetooth (slower connection
  interval); test over USB before changing BLE connection settings. Also check macOS Slow Keys.
- `CONFIG_ZMK_BLE_EXPERIMENTAL_FEATURES=y` is on to fix Mac reconnects (it also turns off 2M PHY).
- The old config (`../corne_eyelash` repo, `markstos` branch) had the same hold-tap Backspace.
  It is retired; don't edit it.
