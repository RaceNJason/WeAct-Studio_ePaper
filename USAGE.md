# Detailed Usage Guide

## Quick Start

1. **Add the external component to your ESPHome configuration:**

```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
```

2. **Configure SPI (required for all ePaper displays):**

```yaml
spi:
  clk_pin: GPIO18
  mosi_pin: GPIO23
```

3. **Configure your display:**

```yaml
display:
  - platform: weact_epaper
    id: epaper_disp
    cs_pin: GPIO5
    dc_pin: GPIO17
    busy_pin: GPIO4
    reset_pin: GPIO16
    model: 2.90in3c
    update_interval: 60s
    lambda: |-
      it.print(0, 0, id(font), "Hello World!")
```

## Pin Configuration

### Required Pins

| Pin | Description | Example |
|-----|-------------|---------|
| `cs_pin` | SPI Chip Select | GPIO5 |
| `dc_pin` | Data/Command | GPIO17 |

### Optional Pins (Highly Recommended)

| Pin | Description | Example | Why Needed |
|-----|-------------|---------|------------|
| `busy_pin` | Busy status | GPIO4 | Allows checking when display is done updating, crucial for power management |
| `reset_pin` | Hardware reset | GPIO16 | Enables proper initialization and recovery |

### SPI Pins (Required via spi: component)

| Pin | Description | Example |
|-----|-------------|---------|
| `clk_pin` | SPI Clock | GPIO18 |
| `mosi_pin` | SPI Data Out | GPIO23 |

**Note:** MISO is not used for ePaper displays.

## Common Pin Configurations

### ESP32 DevKit V1
```yaml
spi:
  clk_pin: GPIO18
  mosi_pin: GPIO23

display:
  - platform: weact_epaper
    cs_pin: GPIO5
    dc_pin: GPIO17
    busy_pin: GPIO4
    reset_pin: GPIO16
```

### ESP8266 NodeMCU
```yaml
spi:
  clk_pin: GPIO14  # D5
  mosi_pin: GPIO13 # D7

display:
  - platform: weact_epaper
    cs_pin: GPIO15   # D8
    dc_pin: GPIO2    # D4
    busy_pin: GPIO4  # D2
    reset_pin: GPIO5 # D1
```

## Display Models

### WeAct Studio Models

#### 2.9" Black/White/Red (296x128)
```yaml
display:
  - platform: weact_epaper
    model: 2.90in3c
    # ... pins ...
```

#### 4.2" Black/White/Red (400x300)
```yaml
display:
  - platform: weact_epaper
    model: 4.20in3c
    # ... pins ...
```

#### 4.2" Black/White (400x300)
```yaml
display:
  - platform: weact_epaper
    model: 4.20in
    # ... pins ...
```

## Advanced Configuration

### Full Update Control

For Type A and Type C displays, you can control how often a full refresh occurs:

```yaml
display:
  - platform: weact_epaper
    model: 2.90in
    full_update_every: 30  # Full refresh every 30 updates
    # ... other config ...
```

**Note:** This option is not available for Type B displays (BWR displays).

### Reset Duration

Adjust the hardware reset pulse duration (default is typically sufficient):

```yaml
display:
  - platform: weact_epaper
    reset_duration: 200ms  # Max 500ms
    # ... other config ...
```

### Display Modes (4.20in-v2 only)

The 4.20in-v2 model supports multiple display modes:

```yaml
display:
  - platform: weact_epaper
    model: 4.20in-v2
    display_mode: FAST  # Options: PARTIAL, FULL, FAST, GRAYSCALE4
    # ... other config ...
```

## Drawing on the Display

### Basic Text

```yaml
font:
  - file: "fonts/arial.ttf"
    id: my_font
    size: 20

display:
  - platform: weact_epaper
    # ... pins and model ...
    lambda: |-
      it.print(10, 10, id(my_font), "Hello World!");
```

### Multi-color (for BWR displays)

```yaml
display:
  - platform: weact_epaper
    model: 2.90in3c  # BWR display
    # ... pins ...
    lambda: |-
      // Black text
      it.print(10, 10, id(my_font), COLOR_ON, "Black Text");
      
      // Red text (on BWR displays, COLOR_OFF draws in the accent color)
      it.print(10, 40, id(my_font), COLOR_OFF, "Red Text");
```

### Drawing Shapes

```yaml
display:
  - platform: weact_epaper
    # ... config ...
    lambda: |-
      // Rectangle
      it.rectangle(10, 10, 100, 50);
      
      // Filled rectangle
      it.filled_rectangle(10, 70, 100, 50);
      
      // Circle
      it.circle(200, 50, 30);
      
      // Line
      it.line(10, 150, 200, 150);
```

### Displaying Sensor Data

```yaml
sensor:
  - platform: dht
    pin: GPIO4
    temperature:
      name: "Temperature"
      id: temp_sensor
    humidity:
      name: "Humidity"
      id: humidity_sensor

display:
  - platform: weact_epaper
    # ... config ...
    lambda: |-
      it.printf(10, 10, id(my_font), "Temp: %.1f°C", id(temp_sensor).state);
      it.printf(10, 40, id(my_font), "Humidity: %.1f%%", id(humidity_sensor).state);
```

## Power Management

### Checking Busy Status

Before putting the ESP into deep sleep, check if the display has finished updating:

```yaml
script:
  - id: prepare_for_sleep
    then:
      - display.page.show: main_page
      - component.update: epaper_disp
      - wait_until:
          lambda: 'return id(epaper_disp).is_display_busy() == 0;'
      - deep_sleep.enter: deep_sleep_control
```

### Deep Sleep Example

```yaml
deep_sleep:
  id: deep_sleep_control
  run_duration: 30s
  sleep_duration: 15min

display:
  - platform: weact_epaper
    # ... config ...
    lambda: |-
      // Draw content
      it.printf(10, 10, id(font), "Awake at: %s", id(homeassistant_time).now().strftime("%H:%M").c_str());
```

## Troubleshooting

### Display not updating
- Verify all pin connections
- Check that `busy_pin` is configured and connected
- Ensure power supply is adequate (3.3V, sufficient current)
- Try adding a longer `update_interval`

### Partial/corrupted display
- Add or check `reset_pin` configuration
- Increase `reset_duration`
- For Type A displays, try adjusting `full_update_every`

### Red color not working (BWR displays)
- Ensure you're using a BWR model (e.g., `2.90in3c`, `4.20in3c`)
- Use `COLOR_OFF` for the red/accent color
- Verify the display hardware supports red (not all WeAct displays do)

### Display shows inverted colors
- This is normal behavior during updates
- The display will show correct colors once the update completes

## GitHub Usage Options

### Latest Version (Recommended for Development)
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
```

### Specific Branch
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper@main
    components: [ weact_epaper ]
```

### Specific Commit (Recommended for Production)
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper@1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p7q8r9s0t  # Replace with actual commit SHA
    components: [ weact_epaper ]
```

### With Refresh Option
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
    refresh: 1d  # Refresh from GitHub once per day
```

## Complete Example

See [`esphome_example.yaml`](esphome_example.yaml) for a complete, ready-to-use example configuration.
