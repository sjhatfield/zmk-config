# Corne 42 LP Wireless - Build & Troubleshooting Notes

## Hardware Configuration
- **Board**: KeebMaker Corne 42 LP Wireless
- **Controller**: nice!nano v2
- **Display**: SSD1306 OLED (Note: Some KeebMaker builds use nice!view, but this specific unit uses an OLED)
- **RGB**: 6 underglow LEDs (WS2812) + per-key LEDs (SK6812). Total 27 LEDs per side.

## Known Issues & Limitations

### 1. OLED Display Orientation
- **The Problem**: The OLED is physically mounted vertically (portrait), but ZMK's built-in display driver renders horizontally (landscape). This causes text to appear sideways (rotated 90°).
- **Attempted Fix**: Using the `nice_oled` module fixes the rotation (it renders to a vertical buffer and rotates 90° in LVGL). 
- **Why it failed**: The `nice_oled` module introduces severe input latency, even when heavy features (animations, WPM tracking, keycode widget) are disabled.
- **Current State**: Using the built-in ZMK basic display. Text is sideways, but there is zero latency.

### 2. RGB LED Chain
- **The Problem**: Only the center column (underglow LEDs) light up. The per-key LEDs do not respond.
- **Attempted Fix**: Adding a custom device tree overlay (`corne.overlay`) configuring SPI3 on P0.06 with a `chain-length` of 27.
- **Why it failed**: The custom overlay broke the I2C bus/OLED functionality, likely due to a pin conflict (P0.06 is UART TX on nice!nano) or incorrect device tree configuration.
- **Current State**: Using default ZMK `CONFIG_ZMK_RGB_UNDERGLOW=y` without overlays. Only the 6 center underglow LEDs work.

### 3. External Power (EXT_POWER) vs OLED
- **The Problem**: ZMK ties the physical power rail of the RGB LEDs to the OLED screen. By default, toggling RGB off also cuts power to the OLED. If the keyboard boots while power is cut, the OLED fails to initialize and stays permanently off.
- **Attempted Fix**: Setting `CONFIG_ZMK_RGB_UNDERGLOW_EXT_POWER=n` and adding an `EP_TOG` key.
- **Why it failed**: We reverted to a clean state to restore basic OLED functionality.
- **Current State**: Do **NOT** press `RGB_TOG` (Nav layer, bottom-left corner). It will kill the OLED until the physical power switch is flipped off and on.

### 4. macOS Modifier Keys Swap
- **The Problem**: macOS detects the ZMK Corne as a non-Apple keyboard and silently swaps the `LGUI` (Command) and `LALT` (Option) keys. The macOS Settings UI does not reflect this swap.
- **The Fix**: The firmware is configured with `LALT` instead of `LGUI`. When ZMK sends `LALT`, macOS interprets it as `Command`, fixing copy/paste (`Cmd+C`/`Cmd+V`).

### 5. Layer-Tap Timing & Quick-Tap
- **The Problem**: Holding a layer-tap key (like Backspace or Enter) to repeat the character would instead activate the layer.
- **The Fix**: Added `quick-tap-ms = <200>` to the `&lt` behavior. Now, tapping a key and immediately holding it within 200ms repeats the keycode instead of triggering the layer.

## Future Steps
An email has been drafted to KeebMaker support requesting their specific ZMK configuration, device tree overlays, and module choices. Once provided, their exact configuration can be applied to fix the RGB chain and display rotation without introducing latency.
