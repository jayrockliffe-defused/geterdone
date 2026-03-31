# Design Integration — Execute Skill

Loaded when a task involves design work, visual assets, or design-dev handoff.

## When to load this file
- Task mentions design, mockup, prototype, visual, layout, UI
- Project involves creating visual assets or implementing designs
- Deliverable is a design file, component, or visual specification

## Figma (if connected)

### Pre-execution
1. `get_metadata` — check file status and last modified
2. `get_design_context` — understand the current design state
3. `search_design_system` — find relevant components and tokens

### During execution
1. `get_screenshot` — capture current design state for reference
2. `get_variable_defs` — get design tokens (colors, spacing, typography)
3. `get_code_connect_suggestions` — get code implementation hints
4. `use_figma` — interact with Figma files as needed

### Post-execution
1. Verify implemented design matches Figma source
2. Check design tokens are correctly applied
3. Flag any deviations for designer review

## Canva (if connected)

### Pre-execution
1. `search-designs` — find relevant existing designs
2. `list-brand-kits` — check brand guidelines
3. `get-design` — load specific design for reference

### During execution
1. `generate-design` — create new designs from prompts
2. `start-editing-transaction` + `perform-editing-operations` — edit existing designs
3. `upload-asset-from-url` — add assets to designs

### Post-execution
1. `export-design` — export final version
2. Verify brand kit compliance
3. Share or publish as needed

## Design-to-code workflow
When implementing a design:
1. Get design tokens from Figma (`get_variable_defs`)
2. Get component specs (`get_design_context`)
3. Map to code components (`get_code_connect_suggestions`)
4. Implement with exact token values (no hardcoded colors/spacing)
5. Screenshot comparison: implementation vs source design

## Important rules
- Always reference design source files in execution logs
- Use design tokens, not hardcoded values
- Flag design-code mismatches as issues, don't silently diverge
- Design tasks are typically semi-auto (Claude implements, user reviews)
