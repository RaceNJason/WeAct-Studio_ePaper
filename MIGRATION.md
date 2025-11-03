# Migration Guide

## Overview

This component has been refactored to be usable as an ESPHome external component via GitHub link. The component has been renamed from `waveshare_epaper` to `weact_epaper` to avoid conflicts with ESPHome's built-in component.

## What Changed

### Directory Structure
- **Old:** `custom_components/waveshare_epaper/`
- **New:** `components/weact_epaper/`

This change follows ESPHome's external component conventions, allowing the component to be used directly from GitHub.

### Component Name
- **Old:** `waveshare_epaper`
- **New:** `weact_epaper`

### Namespace
- **Old:** `namespace waveshare_epaper`
- **New:** `namespace weact_epaper`

## Migration Steps

If you were using a local copy of the old component, follow these steps to migrate:

### Step 1: Update Your YAML Configuration

**Old configuration:**
```yaml
external_components:
  - source: custom_components

display:
  - platform: waveshare_epaper
    # ... rest of config
```

**New configuration (Method 1 - Recommended):**
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]

display:
  - platform: weact_epaper
    # ... rest of config (no other changes needed)
```

**New configuration (Method 2 - Override):**
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
    override: [ waveshare_epaper ]

display:
  - platform: waveshare_epaper  # Can keep old name with override
    # ... rest of config (no other changes needed)
```

### Step 2: Remove Local Copy (Optional)

If you had a local copy of the component, you can now remove it:
```bash
rm -rf custom_components/waveshare_epaper/
```

### Step 3: Recompile

Compile and upload your ESPHome configuration as normal. ESPHome will automatically fetch the component from GitHub.

## Pin Configuration

**No changes required!** All pin configurations remain the same:
- `cs_pin`
- `dc_pin`
- `busy_pin`
- `reset_pin`
- SPI pins (`clk_pin`, `mosi_pin`)

## Model Names

**No changes required!** All model names remain the same:
- `2.90in3c` - WeAct 2.9" BWR
- `4.20in3c` - WeAct 4.2" BWR
- `4.20in` - Waveshare 4.2" BW
- All other Waveshare models

## Benefits of the New Structure

1. **No Manual Installation:** Component is fetched automatically from GitHub
2. **Easy Updates:** Update by changing the commit/branch reference
3. **Version Control:** Pin to specific commits for production stability
4. **No Conflicts:** Renamed component avoids conflicts with built-in ESPHome component
5. **Coexistence:** Can use both this component and ESPHome's built-in waveshare_epaper if needed

## Troubleshooting

### "Platform 'weact_epaper' not found"

Make sure you have the external_components section in your configuration:
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
```

### Want to keep using 'waveshare_epaper' as the platform name?

Use the override approach:
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
    override: [ waveshare_epaper ]
```

### Compilation errors

1. Clear the ESPHome build cache: Delete the `.esphome` directory
2. Try compiling again
3. Check that your pin configurations are correct

## Questions?

For issues or questions, please open an issue at:
https://github.com/Ottes42/WeAct-Studio_ePaper/issues
