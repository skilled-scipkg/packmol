---
name: packmol-index
description: This skill should be used when users ask how to use packmol and the correct generated documentation skill must be selected before going deeper into source code.
---

# packmol Skills Index

## Start in the right directory
- If you start in `skills/`, move to repository root before running Packmol commands: `cd ..`.
- Treat all paths below as repository-root relative (for example `testing/input_files/water_box.inp`).

## Route the request
- Build/install/toolchain/packaging questions: `skills/packmol-build-and-install/SKILL.md`
- Simulation setup, input authoring, execution, and output validation: `skills/packmol-simulation-workflows/SKILL.md`
- Release-history/version-drift/changelog-policy questions: `skills/packmol-changelog/SKILL.md`

## Generated topic skills
- `packmol-build-and-install`: Build and Install (build, installation, compilation, and environment setup)
- `packmol-simulation-workflows`: Simulation Workflows (input templates, run commands, and simulation-start validation)
- `packmol-changelog`: Changelog (documentation grouped under the 'changelog' theme)

## Documentation-first inputs
- `README.md`
- `CHANGELOG.md`
- `testing/input_files`

## Tutorials and examples roots
- `testing/input_files`

## Test roots for behavior checks
- `testing`
- `python/tests`

## Escalate only when needed
- Start from the selected topic skill `SKILL.md`.
- If those references are insufficient, inspect:
  `skills/packmol-build-and-install/references/doc_map.md`, `skills/packmol-simulation-workflows/references/doc_map.md`, or `skills/packmol-changelog/references/doc_map.md`.
- If documentation still leaves ambiguity, inspect:
  `skills/packmol-build-and-install/references/source_map.md`, `skills/packmol-simulation-workflows/references/source_map.md`, or `skills/packmol-changelog/references/source_map.md`.
- Use targeted symbol search while inspecting source (for example: `rg -n "<symbol_or_keyword>" app python src testing`).

## Source directories for deeper inspection
- `app`
- `python`
- `src`
