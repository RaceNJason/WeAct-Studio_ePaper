# Contributing to WeAct Studio ePaper

Thank you for your interest in contributing to the WeAct Studio ePaper component! This guide will help you get started.

## Table of Contents

- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [How to Contribute](#how-to-contribute)
- [Code Style Guidelines](#code-style-guidelines)
- [Testing Your Changes](#testing-your-changes)
- [Submitting Changes](#submitting-changes)
- [Adding New Display Support](#adding-new-display-support)

## Getting Started

This is an ESPHome external component that provides support for WeAct Studio ePaper displays and compatible Waveshare displays. Before contributing, please:

1. Read the [README.md](README.md) to understand the component's purpose
2. Review the [USAGE.md](USAGE.md) to understand how users interact with the component
3. Familiarize yourself with [ESPHome's custom component documentation](https://esphome.io/custom/custom_component.html)

## Development Setup

### Prerequisites

- **ESPHome**: Install ESPHome for testing your changes
  ```bash
  pip install esphome
  ```
- **Git**: For version control
- **Text Editor/IDE**: VSCode, PyCharm, or your preferred editor
- **Hardware** (optional but recommended): An actual ePaper display for testing

### Setting Up Your Development Environment

1. **Fork the Repository**
   - Click the "Fork" button on GitHub
   - Clone your fork:
     ```bash
     git clone https://github.com/YOUR_USERNAME/WeAct-Studio_ePaper.git
     cd WeAct-Studio_ePaper
     ```

2. **Create a Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Test Your Setup**
   - Create a test ESPHome configuration that uses your local component:
   ```yaml
   external_components:
     - source:
         type: local
         path: /path/to/your/WeAct-Studio_ePaper/components
       components: [ weact_epaper ]
   
   display:
     - platform: weact_epaper
       model: 2.90in3c
       # ... other configuration
   ```

## How to Contribute

### Types of Contributions

We welcome various types of contributions:

- **Bug Fixes**: Fix issues with existing display drivers or configurations
- **New Display Support**: Add support for new ePaper display models
- **Documentation**: Improve or clarify documentation
- **Examples**: Add new configuration examples
- **Performance**: Optimize display refresh or power consumption

### Reporting Bugs

If you find a bug, please open an issue with:
- A clear title and description
- Steps to reproduce the issue
- Your ESPHome configuration (sanitized)
- Expected vs actual behavior
- Hardware information (display model, ESP board)
- ESPHome version

### Suggesting Features

For feature requests, please open an issue describing:
- The feature and its benefits
- Use cases
- Potential implementation approach (optional)

## Code Style Guidelines

### Python Code (Configuration Layer)

Follow ESPHome's Python conventions:

```python
# Use proper imports
from esphome import core, pins
import esphome.codegen as cg
import esphome.config_validation as cv

# Use descriptive variable names
CONF_DISPLAY_MODE = "display_mode"

# Follow ESPHome's config validation patterns
CONFIG_SCHEMA = display.FULL_DISPLAY_SCHEMA.extend(
    {
        cv.GenerateID(): cv.declare_id(WaveshareEPaper),
        cv.Required(CONF_MODEL): cv.enum(MODELS, lower=True),
        cv.Optional(CONF_RESET_PIN): pins.gpio_output_pin_schema,
    }
)
```

### C++ Code (Display Drivers)

Follow ESPHome's C++ conventions:

```cpp
// Use proper namespace
namespace esphome {
namespace weact_epaper {

// Clear function names and comments
void WaveshareEPaper::setup() {
  // Initialize hardware
  this->init_internal_(this->get_buffer_length_());
  this->initialize();
}

// Proper logging
void WaveshareEPaper::dump_config() {
  LOG_DISPLAY("", "Waveshare E-Paper", this);
  ESP_LOGCONFIG(TAG, "  Model: %s", this->model_);
}

}  // namespace weact_epaper
}  // namespace esphome
```

### YAML Examples

Keep examples clear and well-commented:

```yaml
# Required SPI configuration
spi:
  clk_pin: GPIO18
  mosi_pin: GPIO23

# Display configuration
display:
  - platform: weact_epaper
    id: epaper_disp
    cs_pin: GPIO5     # Chip select
    dc_pin: GPIO17    # Data/command
    busy_pin: GPIO4   # Busy status (optional but recommended)
    reset_pin: GPIO16 # Hardware reset (optional but recommended)
    model: 2.90in3c   # 2.9" BWR display
    update_interval: 60s
```

## Testing Your Changes

### Local Testing

1. **Configuration Validation**
   - Test that your changes compile:
     ```bash
     esphome compile your_test_config.yaml
     ```

2. **Hardware Testing** (if possible)
   - Upload to actual hardware:
     ```bash
     esphome upload your_test_config.yaml
     ```
   - Verify the display works correctly
   - Test power management (busy pin behavior)
   - For BWR displays, test color rendering

3. **Test Multiple Scenarios**
   - Test with and without optional pins (busy_pin, reset_pin)
   - Test different update intervals
   - Test with different ESPHome versions if possible

### What to Test

- [ ] Configuration compiles successfully
- [ ] Display initializes correctly
- [ ] Display updates work (if hardware available)
- [ ] Busy pin behavior is correct (if hardware available)
- [ ] Documentation is accurate and clear
- [ ] Example configurations are valid
- [ ] No breaking changes to existing configurations

## Submitting Changes

### Before Submitting

1. **Update Documentation**
   - Update README.md if adding new features or displays
   - Update USAGE.md with configuration examples
   - Update MIGRATION.md if making breaking changes
   - Add inline code comments for complex logic

2. **Test Your Changes**
   - Ensure configurations compile
   - Test with hardware if possible
   - Verify backwards compatibility

3. **Review Your Changes**
   - Check for unnecessary changes
   - Remove debug code or comments
   - Ensure code is clean and readable

### Creating a Pull Request

1. **Commit Your Changes**
   ```bash
   git add .
   git commit -m "Add support for [feature/display model]"
   ```

2. **Push to Your Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

3. **Open a Pull Request**
   - Go to the original repository on GitHub
   - Click "New Pull Request"
   - Select your fork and branch
   - Fill out the PR template with:
     - Clear description of changes
     - Why the change is needed
     - Testing performed
     - Any breaking changes
     - Related issues (if any)

### Pull Request Guidelines

- **One feature per PR**: Keep PRs focused on a single change
- **Clear title**: Use descriptive titles (e.g., "Add support for 5.83in display")
- **Complete description**: Explain what, why, and how
- **Update documentation**: Include doc updates in the same PR
- **Reference issues**: Link related issues with "Fixes #123" or "Closes #456"

## Adding New Display Support

To add support for a new ePaper display model:

1. **Identify the Display Specifications**
   - Resolution (width x height)
   - Color support (BW, BWR, or multi-color)
   - Controller chip
   - Initialization sequence

2. **Update `display.py`**
   - Add the model to the `MODELS` dictionary:
   ```python
   "5.83in": WaveshareEPaper5P83In,
   ```
   - Define the model class if needed

3. **Implement Driver (if needed)**
   - Create a new `.cpp` file (e.g., `waveshare_583in.cpp`)
   - Implement initialization and display update methods
   - Handle color modes appropriately

4. **Update Documentation**
   - Add model to README.md supported models list
   - Add configuration example to USAGE.md
   - Include any model-specific notes

5. **Test**
   - Test with actual hardware if possible
   - Verify initialization and display updates work
   - Test with example configurations

6. **Submit PR**
   - Include all changes (code + docs)
   - Describe the display model and testing performed

## Component Dependencies

This component has **no traditional package manager dependencies** (no npm, pip, etc.). 

The only dependencies are:
- **ESPHome component dependencies** declared in `manifest.json`:
  - `spi`: SPI communication
  - `display`: Base display functionality

These are part of the ESPHome framework and managed automatically.

### Why No Dependabot?

This repository does not need Dependabot because:
1. It has no package.json, requirements.txt, or similar dependency files
2. ESPHome component dependencies are framework-level and updated with ESPHome itself
3. Users pin to specific commits/tags of this component for stability

## Dependency Management Strategy

Since this is an ESPHome component:

1. **Component Updates**: Users control which version they use via GitHub URL pinning
2. **ESPHome Compatibility**: We aim to support multiple ESPHome versions
3. **No External Packages**: The component uses only ESPHome's built-in functionality

## Questions?

If you have questions about contributing:
- Open a discussion on GitHub
- Ask in an issue
- Review existing issues and PRs for examples

## Code of Conduct

- Be respectful and inclusive
- Focus on constructive feedback
- Help others learn and grow
- Follow GitHub's community guidelines

## License

By contributing, you agree that your contributions will be licensed under the same license as the project (MIT License, same as ESPHome).

Thank you for contributing! 🎉
