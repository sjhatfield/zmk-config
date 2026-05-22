# ZMK Config — Corne 42 LP Wireless

Custom ZMK firmware for a [KeebMaker Corne 42 LP](https://keebmaker.com/products/corne-low-profile) wireless split keyboard with nice!nano v2 controllers.

## Keymap

### Layer 0 — Base
```
 ESC  |  Q  |  W  |  E  |  R  |  T  │  Y  |  U  |  I  |  O  |  P  | DEL
 GUI  |  A  |  S  |  D  |  F  |  G  │  H  |  J  |  K  |  L  |  ;  |  '
 SHF  |  Z  |  X  |  C  |  V  |  B  │  N  |  M  |  ,  |  .  |  /  | CTL
                CTL | LT1(Ent) | LT2(Spc) │ LT2(Bksp) | LT1(Tab) | CTL
```

### Layer 1 — Navigation / Arrows
```
BT_CLR|     |     |     |     |     │     | Vol↓| Vol↑|  {  |  }  | DEL
      | BT0 | BT1 | BT2 | BT3 | BT4 │  ←  |  ↓  |  ↑  |  →  |     |
RGB_T | HUE↓| HUE↑| BRI↓| BRI↑| EFF │     | PgDn| PgUp|     |     |
                ▽  |     |  ▽        │  ▽  |  ▽  |  ▽
```

### Layer 2 — Numbers / Symbols
```
  `   |  !  |  @  |  #  |  $  |  %  │  ^  |  &  |  *  |  (  |  )  | DEL
      |  1  |  2  |  3  |  4  |  5  │  6  |  7  |  8  |  9  |  0  |
      |     |     |     |  =  |  -  │     |     |     |     |  \  |
                ▽  |  ▽  |  ▽        │  ▽  |     |  ▽
```

`▽` = transparent (passthrough to base layer)

## Features

- **Quick-tap** (200ms) on all layer-tap keys — tap then hold repeats the key instead of activating the layer. Great for backspace/enter/space.
- **Bluetooth** — 5 profiles selectable from nav layer (BT0–BT4), BT_CLR in top-left corner.
- **RGB underglow** controls on nav layer bottom row.
- **ZMK Studio** support enabled for live editing via USB.

## Building

Firmware is built automatically by GitHub Actions on every push. Download the `.uf2` artifacts from the [Actions tab](https://github.com/sjhatfield/zmk-config/actions).

## Flashing

1. Double-press the reset button on a half → USB drive appears
2. Drag the matching `.uf2` file onto it
3. Repeat for the other half

**Do NOT use a TRRS cable** with nice!nano controllers — the halves communicate over Bluetooth.
