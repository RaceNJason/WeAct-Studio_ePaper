# Dependency Management

## Overview

This document explains the dependency management strategy for the WeAct Studio ePaper component.

## TL;DR - No Traditional Dependencies

**This repository does NOT require Dependabot or traditional dependency management tools.**

This is an ESPHome external component with no package manager dependencies (npm, pip, cargo, etc.). All dependencies are ESPHome framework-level components.

## Why No Traditional Dependencies?

### Repository Type
This is an **ESPHome external component**, not a standalone application or library. It provides display drivers that integrate with the ESPHome framework.

### How It's Consumed
Users consume this component by referencing it in their ESPHome configurations:

```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
```

ESPHome handles downloading and integrating the component during the build process.

## Component Dependencies

### Declared in manifest.json

The only dependencies are ESPHome component dependencies declared in `components/weact_epaper/manifest.json`:

```json
{
  "name": "weact_epaper",
  "codeowners": ["@clydebarrow"],
  "dependencies": ["spi", "display"],
  "documentation": "https://github.com/Ottes42/WeAct-Studio_ePaper"
}
```

### What These Dependencies Are

- **`spi`**: ESPHome's SPI communication component (built into ESPHome)
- **`display`**: ESPHome's base display functionality (built into ESPHome)

These are **NOT** external packages that need version management. They are:
- Part of the ESPHome framework itself
- Updated when ESPHome is updated
- Maintained by the ESPHome project
- Not managed through package managers

## Files That Don't Exist (And Why That's Okay)

This repository intentionally does NOT have:

| File | Why It Doesn't Exist |
|------|---------------------|
| `package.json` | No Node.js/npm dependencies |
| `requirements.txt` | No Python package dependencies (uses ESPHome's built-in modules) |
| `Cargo.toml` | Not a Rust project |
| `go.mod` | Not a Go project |
| `Gemfile` | Not a Ruby project |
| `pom.xml` | Not a Maven project |
| `build.gradle` | Not a Gradle project |
| `composer.json` | Not a PHP project |

## Dependency Management Strategy

### For This Component

1. **ESPHome Version Compatibility**
   - We aim to support multiple ESPHome versions
   - Breaking changes in ESPHome are handled as they occur
   - No automatic dependency updates needed

2. **Component Structure**
   - Follow ESPHome's component API
   - Use ESPHome's built-in modules only
   - No external package dependencies

3. **User Version Control**
   - Users pin to specific commits/tags for stability
   - Users control when to update by changing their GitHub reference
   - No forced updates through dependency managers

### For Users

Users control which version of this component they use:

**Latest version (automatic updates):**
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
```

**Specific branch:**
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper@main
    components: [ weact_epaper ]
```

**Pinned to commit (recommended for production):**
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper@abc123def456
    components: [ weact_epaper ]
```

**Refresh interval:**
```yaml
external_components:
  - source: github://Ottes42/WeAct-Studio_ePaper
    components: [ weact_epaper ]
    refresh: 1d  # Check for updates daily
```

## Why Dependabot Is Not Needed

### Dependabot Use Cases
Dependabot is typically used for:
1. Updating package dependencies (npm, pip, bundler, etc.)
2. Security vulnerability notifications
3. Automated dependency updates

### Why It Doesn't Apply Here

1. **No Package Files**
   - Dependabot scans files like `package.json`, `requirements.txt`, etc.
   - This repository has none of these files
   - Nothing for Dependabot to scan or update

2. **Framework-Level Dependencies**
   - Our dependencies are ESPHome components
   - These are not versioned separately from ESPHome
   - They're part of ESPHome's core framework

3. **User-Controlled Updates**
   - Users choose when to update this component
   - Updates happen by changing the GitHub URL reference
   - No automated dependency update mechanism is needed or desired

## Security Considerations

### How Security Is Managed

1. **Code Review**
   - All changes go through pull request review
   - Security issues are addressed in code reviews

2. **ESPHome Framework Security**
   - Security for SPI and display components is handled by ESPHome
   - ESPHome project manages framework-level security

3. **No External Dependencies**
   - Reduced attack surface by not using external packages
   - No supply chain attacks through npm/pip/etc.

### Vulnerability Scanning

While Dependabot isn't needed for dependency updates, GitHub's security features still provide:
- Code scanning (if enabled)
- Secret scanning (if enabled)
- Security advisories (if needed)

These operate independently of Dependabot and package dependencies.

## Monitoring for Updates

### ESPHome Framework Updates

Monitor ESPHome releases for:
- API changes that might affect this component
- New features that could be leveraged
- Deprecations that need to be addressed

**ESPHome Release Page**: https://github.com/esphome/esphome/releases

### Display Hardware Updates

Monitor manufacturer documentation for:
- New display models
- Updated initialization sequences
- Hardware errata

## Conclusion

**This repository does not need Dependabot or similar dependency management tools** because:

1. ✅ It has no package manager dependencies
2. ✅ ESPHome component dependencies are framework-level
3. ✅ Users control their own update schedule via GitHub URL pinning
4. ✅ The component is self-contained and uses only ESPHome's built-in functionality

This is the correct and intentional architecture for an ESPHome external component.

## Further Reading

- [ESPHome External Components Documentation](https://esphome.io/components/external_components.html)
- [ESPHome Custom Components Guide](https://esphome.io/custom/custom_component.html)
- [ESPHome Development Guide](https://esphome.io/guides/contributing.html)
