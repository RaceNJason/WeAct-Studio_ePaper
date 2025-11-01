# Upstream Sources and Fork Relationship

## Overview

This document explains the relationship between this repository and its upstream sources, and discusses auto-merge possibilities.

## Repository Type

**This is NOT a traditional fork of a single upstream repository.**

This repository is a **curated collection and integration** of code from multiple sources, assembled into an ESPHome external component.

## Source Attribution

As documented in the README.md, this component builds upon work from multiple sources:

### Primary Sources

1. **MrMDavidson's Work**
   - Original WeAct Studio 4.2" BW screen implementation
   - ESPHome PR #6209
   - Contribution: 4.2" Black/White display driver

2. **jbergler's Work**
   - Original WeAct Studio 2.9" BWR screen implementation
   - ESPHome PR #6226
   - Contribution: 2.9" Black/White/Red display driver

3. **ESPHome Team**
   - Base waveshare_epaper component implementation
   - ESPHome core repository: https://github.com/esphome/esphome
   - Contribution: Base component structure and various Waveshare display drivers

### Integration Work

This repository combines these sources into:
- A unified external component
- Support for both WeAct Studio and Waveshare displays
- A renamed component (`weact_epaper`) to avoid conflicts
- Easy consumption via GitHub URL

## Not a GitHub Fork

### Why This Isn't a Fork

GitHub's fork feature is used for:
1. Contributing back to the original repository
2. Tracking divergence from a single upstream
3. Creating pull requests to the parent repository

This repository:
- ❌ Does not have a single parent repository on GitHub
- ❌ Is not marked as a fork in GitHub's metadata
- ❌ Cannot use GitHub's fork update mechanisms
- ✅ Is an independent repository that credits multiple sources
- ✅ Consolidates code from multiple ESPHome PRs and components

### How to Verify

You can verify this is not a GitHub fork by:

1. **Check GitHub Repository Page**
   - No "forked from" indicator at the top
   - No "Fetch upstream" button

2. **Check via GitHub API** (requires authentication)
   ```bash
   curl -H "Authorization: token YOUR_TOKEN" \
     https://api.github.com/repos/Ottes42/WeAct-Studio_ePaper | \
     jq '.fork, .parent'
   ```
   
   Expected result: `"fork": false`, `"parent": null`

3. **Check Git Remotes**
   ```bash
   git remote -v
   ```
   
   Expected result: Only `origin` pointing to `Ottes42/WeAct-Studio_ePaper`

## Auto-Merge Considerations

### Can Auto-Merge Be Enabled?

**No, auto-merge from upstream cannot be enabled** for the following reasons:

### Why Auto-Merge Isn't Applicable

1. **No Single Upstream Repository**
   - Code comes from multiple sources (ESPHome PRs, ESPHome core)
   - No single repository to "auto-merge" from
   - Each source has its own development timeline

2. **Different Code Paths**
   - MrMDavidson's and jbergler's PRs were proposed to ESPHome
   - Those PRs may or may not be merged into ESPHome core
   - If merged, they would be in ESPHome's built-in component
   - This component exists as an alternative external component

3. **ESPHome Core Divergence**
   - ESPHome's built-in `waveshare_epaper` continues to evolve
   - This component is renamed to `weact_epaper` for coexistence
   - Namespaces and structure are intentionally different
   - Direct merge would cause conflicts

4. **Independent Evolution**
   - This repository may add features not in ESPHome
   - ESPHome may add features not in this repository
   - Both can coexist and serve different needs

### Manual Update Strategy

Instead of auto-merge, updates should be done **manually and selectively**:

#### From ESPHome Core

1. **Monitor ESPHome Releases**
   - Watch: https://github.com/esphome/esphome/releases
   - Look for relevant waveshare_epaper changes

2. **Evaluate Changes**
   - Review if changes apply to this component
   - Consider if changes improve functionality
   - Check for compatibility with existing users

3. **Port Changes**
   - Manually port relevant improvements
   - Adapt to `weact_epaper` namespace
   - Test thoroughly before releasing

#### From Original PRs

1. **Monitor PR Authors' Repositories**
   - Check if MrMDavidson or jbergler publish updates
   - Review their ESPHome PR discussions

2. **Incorporate Improvements**
   - Pull in bug fixes if identified
   - Add new features if applicable

## Tracking Upstream Changes

### Recommended Approach

1. **Watch ESPHome Repository**
   - GitHub watch for releases
   - Subscribe to relevant issue/PR notifications

2. **Check Periodically**
   - Review ESPHome's waveshare_epaper component changes quarterly
   - Check for security updates or bug fixes

3. **Document Changes**
   - When porting changes, document the source
   - Credit original authors in commits
   - Update this document with sources

### Creating a Manual Sync Process

If you want to periodically review ESPHome changes:

```bash
# Add ESPHome as a reference remote (do not merge)
git remote add esphome https://github.com/esphome/esphome.git
git fetch esphome

# View recent changes to their waveshare_epaper component
git log esphome/dev -- esphome/components/waveshare_epaper/

# View differences (for manual porting)
git diff HEAD esphome/dev -- esphome/components/waveshare_epaper/
```

**Important**: Do NOT merge directly. Review and port changes manually.

## GitHub Actions for Update Notifications

### Possible Automation

While auto-merge isn't feasible, you could create a GitHub Action to:

1. **Notify of ESPHome Releases**
   - Check ESPHome releases via API
   - Create an issue when new release is detected
   - Prompt manual review of changes

2. **Compare Versions**
   - Periodically fetch ESPHome's waveshare_epaper code
   - Generate a diff report
   - Create an issue with changes to review

Example workflow (pseudocode):
```yaml
name: Check ESPHome Updates

on:
  schedule:
    - cron: '0 0 * * 0'  # Weekly on Sunday

jobs:
  check-updates:
    runs-on: ubuntu-latest
    steps:
      - name: Check ESPHome latest release
      - name: Compare with last checked version
      - name: Create issue if new changes detected
```

**Note**: This would only provide notifications, not automatic merging.

## Contributing Updates

If you identify useful changes from upstream sources:

1. **Create an Issue**
   - Describe the upstream change
   - Explain why it should be ported
   - Link to the original source

2. **Submit a Pull Request**
   - Port the changes manually
   - Test thoroughly
   - Credit the original author
   - Reference the upstream source

## Relationship to ESPHome Project

### Complementary, Not Competitive

- **ESPHome built-in**: Official waveshare_epaper component
- **This component**: External alternative with WeAct Studio focus
- **Coexistence**: Both can be used simultaneously if needed

### When to Use This Component

Use this component when you:
- Have a WeAct Studio display
- Need features not yet in ESPHome's built-in component
- Want to avoid conflicts with the built-in component
- Need specific display models supported here

### When to Use ESPHome Built-in

Use ESPHome's built-in component when:
- You prefer official ESPHome components
- Your display is well-supported by the built-in component
- You want the most stable/tested option

## Summary

| Question | Answer |
|----------|--------|
| Is this a GitHub fork? | No |
| Can auto-merge be enabled? | No |
| Why not? | Multiple upstream sources, intentional differences |
| How to track upstream? | Manual monitoring and selective porting |
| Can both components coexist? | Yes, different namespaces |

## Conclusion

**Auto-merge from upstream is not applicable** to this repository because:

1. ✅ Not a traditional fork of a single repository
2. ✅ Integrates code from multiple sources
3. ✅ Uses different namespace to enable coexistence
4. ✅ Requires manual evaluation of upstream changes
5. ✅ Serves as an independent external component option

The recommended approach is **manual monitoring and selective porting** of relevant upstream improvements.

## Further Reading

- [ESPHome External Components](https://esphome.io/components/external_components.html)
- [ESPHome Contributing Guide](https://esphome.io/guides/contributing.html)
- [GitHub Fork Documentation](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks)
