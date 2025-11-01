# WeAct Studio ePaper Component for ESPHome

ESPHome external component for WeAct Studio ePaper screens, including support for select Waveshare ePaper displays.

## Supported Displays

This component covers:

- **WeAct Studio 2.9" BWR screen** (296x128) - Based on ESPHome pull request #6226 by jbergler
- **WeAct Studio 4.2" BW screen** (400x300) - Based on ESPHome pull request #6209 by MrMDavidson
- **WeAct Studio 4.2" BWR screen** (400x300) - Custom implementation based on jbergler's work
- **Various Waveshare ePaper displays** - Latest ESPHome waveshare_epaper component code

## Installation

### Method 1: Using GitHub Link (Recommended)

Add the following to your ESPHome YAML configuration:

```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
```

### Method 2: Local Installation

1. Clone this repository or download the `components` folder
2. Copy the `components/weact_epaper` folder into your ESPHome configuration directory
3. Reference it in your YAML:

```yaml
external_components:
  - source: components
    components: [ weact_epaper ]
```

## Configuration

### Basic Setup

```yaml
# SPI Configuration (required)
spi:
  clk_pin: GPIO18
  mosi_pin: GPIO23

# Display Configuration
display:
  - platform: weact_epaper
    id: epaper_disp
    cs_pin: GPIO5
    dc_pin: GPIO17
    busy_pin: GPIO4      # Optional but recommended
    reset_pin: GPIO16    # Optional but recommended
    model: 2.90in3c      # Choose your model
    update_interval: 60s
    lambda: |-
      it.print(0, 0, id(font), "Hello World!")
```

### Supported Models

#### WeAct Studio Models
- `2.90in3c` - 2.9" Black/White/Red (296x128)
- `4.20in3c` - 4.2" Black/White/Red (400x300)
- `4.20in` - 4.2" Black/White (400x300)

#### Waveshare Models
This component also supports many Waveshare ePaper displays. See the complete list in `components/weact_epaper/display.py`.

Common models include:
- `1.54in`, `1.54inv2`
- `2.13in`, `2.13inv2`, `2.13inv3`
- `2.70in`, `2.70inv2`
- `2.90in`, `2.90inv2`
- `4.20in-bv2`, `4.20in-v2`
- `5.83in`, `5.83inv2`
- `7.50in`, `7.50inv2`, `7.50in-bv2`, `7.50in-bv3`
- And many more...

### Configuration Options

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `platform` | string | yes | Must be `weact_epaper` |
| `model` | string | yes | Display model (see list above) |
| `cs_pin` | pin | yes | SPI chip select pin |
| `dc_pin` | pin | yes | Data/Command pin |
| `reset_pin` | pin | no | Reset pin (recommended) |
| `busy_pin` | pin | no | Busy pin (recommended for sleep optimization) |
| `update_interval` | time | no | How often to update display (default: 1s) |
| `full_update_every` | int | no | Number of updates before full refresh (Type A/C only) |
| `reset_duration` | time | no | Reset pulse duration (max 500ms) |

### Complete Example

See [`esphome_example.yaml`](esphome_example.yaml) for a complete working example.

## Power Management

The `busy_pin` configuration is highly recommended as it allows for efficient sleep code to maximize battery life. Without it, entering sleep mode before the display has finished updating will cause the display to stop mid-update.

You can check if the display is busy in your code:
```cpp
if (id(epaper_disp).is_display_busy() == 0) {
  // Display is not busy, safe to sleep
}
```

## Credits

This component builds upon work from:
- **MrMDavidson** - Original WeAct Studio 4.2" BW screen implementation (ESPHome PR #6209)
- **jbergler** - Original WeAct Studio 2.9" BWR screen implementation (ESPHome PR #6226)
- **ESPHome Team** - Base waveshare_epaper component implementation

## Component Naming

This component is named `weact_epaper` (instead of `waveshare_epaper`) to:
1. Avoid conflicts with ESPHome's built-in waveshare_epaper component
2. Clearly indicate support for WeAct Studio displays
3. Allow both components to coexist if needed

## Contributing

Contributions are welcome! If you have support for additional ePaper displays or improvements, please submit a pull request.

## License

This component follows the same license as ESPHome (MIT License).
