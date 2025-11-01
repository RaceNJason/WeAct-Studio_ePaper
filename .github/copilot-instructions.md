# GitHub Copilot Instructions for WeAct Studio ePaper

## Project Overview

This repository provides an ESPHome external component for WeAct Studio ePaper displays and compatible Waveshare ePaper displays. The component allows ESPHome users to easily integrate ePaper displays into their smart home projects.

## Repository Structure

```
WeAct-Studio_ePaper/
├── components/
│   └── weact_epaper/          # ESPHome component implementation
│       ├── __init__.py        # Component registration (Python)
│       ├── display.py         # Component configuration (Python)
│       ├── manifest.json      # Component metadata and dependencies
│       ├── *.cpp              # Display driver implementations (C++)
│       └── *.h                # Header files (C++)
├── README.md                  # Main documentation
├── USAGE.md                   # Detailed usage guide
├── MIGRATION.md               # Migration guide from older versions
├── esphome_example.yaml       # Example ESPHome configuration
└── .gitignore                 # Git ignore patterns
```

## Technology Stack

### Languages
- **C++**: Display driver implementations
- **Python**: ESPHome component configuration and integration layer
- **YAML**: ESPHome configuration examples

### Framework
- **ESPHome**: This is an ESPHome external component, not a standalone application
- The component is consumed by ESPHome configurations via GitHub URL
- No build system or traditional dependencies like npm, pip, etc.

## Component Dependencies

This component has **ESPHome component dependencies** declared in `manifest.json`:
- `spi`: SPI communication protocol support
- `display`: Base display functionality

These are NOT package manager dependencies but ESPHome component dependencies that are part of the ESPHome framework itself.

## Development Guidelines

### Making Code Changes

#### Python Files (`__init__.py`, `display.py`)
- These files define the ESPHome component configuration schema
- Follow ESPHome's configuration validation patterns using `config_validation` (cv)
- Use `codegen` (cg) for code generation
- Maintain backwards compatibility with existing configurations
- Add new display models by extending the `MODELS` dictionary in `display.py`

#### C++ Files (`.cpp`, `.h`)
- Follow ESPHome's C++ coding style
- Implement display drivers by inheriting from base classes
- Keep hardware-specific code isolated to individual model implementations
- Use proper initialization and cleanup in `setup()` and `dump_config()`
- Handle busy pin checking for power management optimization

#### Example Configuration (`esphome_example.yaml`)
- Keep examples simple and well-commented
- Show the most common use case
- Include pin configuration examples for popular boards (ESP32, ESP8266)

### Documentation

#### When to Update Documentation
- **README.md**: Changes to supported displays, installation methods, or high-level features
- **USAGE.md**: Changes to configuration options, pin assignments, or usage examples
- **MIGRATION.md**: Breaking changes or significant refactoring that affects users
- **Component files**: Update inline comments for complex logic

#### Documentation Style
- Use clear, concise language
- Provide complete code examples
- Include pin configuration tables
- Show both required and optional configurations
- Document any hardware-specific quirks

### Adding New Display Support

1. Add display model definition in `display.py` to the `MODELS` dictionary
2. Implement the display driver in a new `.cpp` file if needed
3. Update the model list in `README.md`
4. Add example configuration in `USAGE.md`
5. Test with actual hardware if possible

### Code Style

#### Python (ESPHome Configuration)
```python
# Use ESPHome's code generation patterns
import esphome.codegen as cg
import esphome.config_validation as cv

# Follow ESPHome naming conventions
CONFIG_SCHEMA = display.FULL_DISPLAY_SCHEMA.extend(
    {
        cv.GenerateID(): cv.declare_id(WaveshareEPaper),
        cv.Required(CONF_MODEL): cv.enum(MODELS, lower=True),
        # ... other fields
    }
).extend(spi.spi_device_schema())
```

#### C++ (Display Drivers)
```cpp
// Follow ESPHome namespace conventions
namespace esphome {
namespace weact_epaper {

// Use clear, descriptive function names
void WaveshareEPaper::setup() {
  // Hardware initialization
}

void WaveshareEPaper::dump_config() {
  // Configuration logging
}

}  // namespace weact_epaper
}  // namespace esphome
```

### Testing

#### Manual Testing
Since this is a hardware component:
1. Test with actual ePaper displays when possible
2. Verify SPI communication works correctly
3. Check power management (busy pin behavior)
4. Test display updates and refresh cycles
5. Validate color support on BWR displays

#### Configuration Testing
1. Validate YAML configuration examples compile successfully
2. Test with different ESPHome versions
3. Verify external component loading from GitHub works

### Common Pitfalls to Avoid

1. **Breaking Configuration Compatibility**: Always maintain backwards compatibility with existing user configurations
2. **Missing Busy Pin Handling**: The busy pin is critical for power management; don't skip it
3. **Incorrect Color Handling**: BWR displays use different rendering than BW displays
4. **SPI Timing Issues**: Follow manufacturer specifications for SPI communication
5. **Namespace Conflicts**: Always use the `weact_epaper` namespace to avoid conflicts with ESPHome's built-in components

## Component Naming Convention

This component is named `weact_epaper` instead of `waveshare_epaper` to:
- Avoid conflicts with ESPHome's built-in waveshare_epaper component
- Clearly indicate support for WeAct Studio displays
- Allow both components to coexist if needed

## Git Workflow

### Branches
- `main`: Stable branch for production use
- Feature branches: Use descriptive names (e.g., `add-display-model-xyz`)

### Commits
- Write clear commit messages describing what and why
- Keep commits focused on a single change
- Reference issue numbers when applicable

### Pull Requests
- Include a clear description of changes
- Update relevant documentation
- Test configurations compile successfully
- Add example configurations for new features

## User Support

### Common User Issues
1. **"Platform 'weact_epaper' not found"**: User needs to add external_components section
2. **Display not updating**: Usually a wiring or busy pin issue
3. **Inverted colors**: Normal during update cycle
4. **Red color not working**: Must use COLOR_OFF on BWR displays

### Where Users Get Help
- GitHub Issues for bug reports and feature requests
- Discussions for usage questions
- README.md and USAGE.md for self-service documentation

## ESPHome Integration

### How Users Consume This Component

Users add this to their ESPHome YAML configuration:
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
```

ESPHome then:
1. Downloads the component from GitHub
2. Integrates it into the build process
3. Generates C++ code from the configuration
4. Compiles firmware for the ESP32/ESP8266

### Version Pinning

Users can pin to specific versions:
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper@main
    components: [ weact_epaper ]
```

Or specific commits for production stability:
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper@abc123def
    components: [ weact_epaper ]
```

## Contributing Reminders

- This is an ESPHome component, not a standalone application
- Changes must work within ESPHome's component framework
- Test with actual hardware when possible
- Maintain backwards compatibility
- Update documentation for all user-facing changes
- Follow ESPHome's coding conventions and patterns

## Security Considerations

- No secrets or credentials should be committed
- Example configurations should use placeholder values
- SPI communication should follow secure coding practices
- Validate all user inputs from YAML configuration

## Additional Resources

- [ESPHome Documentation](https://esphome.io/)
- [ESPHome Custom Components Guide](https://esphome.io/custom/custom_component.html)
- [WeAct Studio Hardware Documentation](https://github.com/WeActStudio)
- [Waveshare ePaper Documentation](https://www.waveshare.com/wiki/E-Paper)
