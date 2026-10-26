# MODEUS UI ASSET PACK v1

## Files
- `dialogue_frame.svg`: Dialogue visual frame, with MODEUS label. Keep text in HTML.
- `command_frame.svg`: Current command visual frame, with label. Keep command title and description in HTML.
- `hud_frame.svg`: Reusable scalable frame WITHOUT text, suitable for HUD panels.
- `progress_track.svg`: Empty progress bar track. Dynamic fill must be HTML/CSS.
- `trial_slot_empty.svg`, `trial_slot_complete.svg`: Separate 70x70 trial markers.
- `button_idle.svg`, `button_pressed.svg`, `button_primary.svg`: Visual button states. Real buttons must be HTML `<button>`.
- `diamond.svg`: Decorative icon.
- `demo.html`: Working static demonstration of responsive integration. Values are examples ONLY.

## Critical integration instructions for Grok Bot
Repository: yu10708c-bot/modeus
Entry point: modeus-edge.html

1. Inspect the current repository and existing gameplay state/events BEFORE editing.
2. Upload the contents of this package to `assets/ui/` preserving exact filenames.
3. Preserve original Modeus character images and all game logic.
4. Do not crop any sprite sheets: all supplied SVGs are separate assets.
5. Render dialogue, command, BPM, lust %, countdown and trial numbers as LIVE HTML text. Never embed gameplay values in images.
6. `hud_frame.svg` can be applied as a nine-slice border-image:
   border: 28px solid transparent;
   border-image-source: url("assets/ui/hud_frame.svg");
   border-image-slice: 40 fill;
   border-image-width: 28px;
   border-image-repeat: stretch;
   background: #100c12;
   box-sizing: border-box;
   The text and progress bar are children of the panel, not part of the SVG.
7. `dialogue_frame.svg` and `command_frame.svg` are decorative reference assets with fixed label plates. For variable-height content prefer the `hud_frame.svg` nine-slice plus a REAL HTML label over it, matching the supplied design. This avoids stretching the baked label.
8. `progress_track.svg` is a background layer; put an absolutely positioned fill element inside it with width based on actual state. Do not draw percentages into the SVG.
9. Render exactly 3/6/9 trial slots based on current difficulty. Use empty or complete slot SVG per the existing completed-trials count. Display a single live `completed / required` label.
10. Buttons must remain semantic HTML buttons, using SVGs as background decoration. Preserve existing handlers and tactile feedback.
11. Use `pointerdown`/`pointerup` visual pressed state without triggering the gameplay action twice. Existing click/game handlers remain the source of truth.
12. Only redesign the command phase first. Do not alter trial, penalty, final challenge, save state, dialogue cadence, BPM, or game balance.
13. Verify on mobile-sized viewports and inspect for CSS overrides, image errors, overflow, and tap behavior. Do not claim physical Safari testing unless performed.
14. Report exact changed files and any limitations. Commit only after checking the implementation.

## Responsive design recommendation
Use one column on mobile. Keep character artwork large, then dialogue, BPM and trial progress, command, lust progress, timer, surrender. Use `hud_frame.svg` for resizable containers; `dialogue_frame.svg` and `command_frame.svg` can be used in static-aspect contexts, but do not distort them to accommodate arbitrarily long text.

## CSS example
```css
.modeus-panel {
  border: 28px solid transparent;
  border-image: url("assets/ui/hud_frame.svg") 40 fill / 28px / 0 stretch;
  background: #100c12;
  color: #f4eee9;
  box-sizing: border-box;
  min-width: 0;
}
.modeus-progress {
  position: relative;
  background: #251d26 url("assets/ui/progress_track.svg") center / 100% 100% no-repeat;
  height: 22px;
  overflow: hidden;
}
.modeus-progress > .fill {
  position: absolute;
  left: 5px;
  top: 5px;
  bottom: 5px;
  background: #f03740;
  width: var(--progress, 0%);
  max-width: calc(100% - 10px);
}
.modeus-trial-slot { width: 30px; height: 30px; object-fit: contain; }
.modeus-button {
  background: url("assets/ui/button_idle.svg") center / 100% 100% no-repeat;
  border: 0;
  color: #f4eee9;
  min-height: 54px;
  touch-action: manipulation;
  transition: transform 90ms ease, filter 90ms ease;
}
.modeus-button.is-pressed,
.modeus-button:active {
  background-image: url("assets/ui/button_pressed.svg");
  transform: translateY(4px) scale(.98);
}
```
