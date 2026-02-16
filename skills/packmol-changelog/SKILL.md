---
name: packmol-changelog
description: This skill should be used when users ask about changelog in packmol; it prioritizes documentation references and then source inspection only for unresolved details.
---

# packmol: Changelog

## High-Signal Playbook

### Route conditions
- Use this skill for release-history questions, upgrade impact, and version-to-version behavior deltas.
- Route install/compilation questions to `packmol-build-and-install`.
- Route simulation setup and runtime behavior questions to `packmol-simulation-workflows`.

### Triage questions
- Which version transition matters (from X to Y), and is `-DEV` acceptable or only tagged releases? (`CHANGELOG.md`)
- Is the request about user-facing flags/options, CI/release process, or internal bug fixes?
- Which keywords/features are implicated (for example `ignore_conect`, `non_standard_conect`, `-i -o` CLI)? (`CHANGELOG.md`, `src/getinp.f90`, `src/cli_parser.f90`)
- Does the user need a release-note summary or a code-level verification of behavior change?
- Are version numbers synchronized across release metadata files? (`fpm.toml`, `pyproject.toml`, `src/title.f90`)
- Is the change being prepared in a PR that must satisfy changelog enforcement? (`.github/workflows/changelog.yml`)

### Canonical workflow
1. Read `CHANGELOG.md` and isolate entries between the baseline and target versions.
2. Classify each relevant entry as feature/enhancement/bugfix/info and identify user-facing risk.
3. Map each changelog item to implementation files with targeted token search (`rg`) before making behavior claims.
4. For release prep, run `./update_version.sh <version>` to synchronize version strings in source/package metadata.
5. Manually add/update the changelog section; `update_version.sh` does not edit `CHANGELOG.md`.
6. Validate CI constraints (`.github/workflows/changelog.yml`) and run targeted behavior checks tied to changed features.
7. Report upgrade notes with explicit file-path evidence.

### Minimal working example
- Trace a changelog feature to implementation and tests:
```bash
rg -n "Version 21.2.0|ignore_conect|non_standard_conect" CHANGELOG.md src/getinp.f90
rg -n "parse_command|parse_command_line_args|-i|-o" src/cli_parser.f90
rg -n "CONECT|conect|connectivity|non_standard_conect" testing/test_connectivity.sh testing/input_files
```

- Sync release version metadata and verify:
```bash
./update_version.sh 21.2.2
rg -n 'version = "21.2.2"' fpm.toml pyproject.toml
rg -n "Version 21.2.2" src/title.f90
rg -n "Version 21.2.2" CHANGELOG.md
```

### Pitfalls and fixes
- PR blocked by missing changelog update: update `CHANGELOG.md` or explicitly use the documented skip-label process. (`.github/workflows/changelog.yml`)
- Version drift across files: use `update_version.sh` and re-check `fpm.toml`, `pyproject.toml`, `src/title.f90` for consistent version strings.
- Assuming `update_version.sh` updates release notes: it only updates version fields and prints commit range hints; edit `CHANGELOG.md` manually.
- Using outdated script name (release.sh, legacy name): use `update_version.sh` (noted in changelog history).
- Overstating behavior from changelog-only text: map to source/tests before concluding runtime semantics.
- Treating `21.2.2-DEV` as a final release: verify whether the target is a development or released tag.
- `testing/test_connectivity.sh` may fail on connectivity input naming mismatch; use `testing/input_files/water_box_conect.inp` for manual checks when that happens.

### Convergence and validation checks
- Changelog has a correctly ordered section for the target version (`CHANGELOG.md`).
- Version strings are synchronized across `fpm.toml`, `pyproject.toml`, and `src/title.f90`.
- Changelog enforcement workflow remains active for PRs (`.github/workflows/changelog.yml`).
- Behavior-linked checks pass for changed areas (for example CLI: `testing/test_cli.sh`; connectivity input semantics: `testing/input_files/water_box_conect.inp`).

## Scope
- Handle questions about documentation grouped under the 'changelog' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `CHANGELOG.md`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `testing/input_files`

## Test references
- `testing`
- `python/tests`

## Optional deeper inspection
- `app`
- `python`
- `src`
- `.github/workflows`

## Source entry points for unresolved issues
- `CHANGELOG.md` (authoritative release notes and version sections)
- `update_version.sh` (release-version synchronization workflow and `sed` replacements)
- `src/title.f90` (`title` subroutine; version string shown by Packmol banner/title)
- `fpm.toml` (Fortran package version metadata)
- `pyproject.toml` (Python package version metadata)
- `.github/workflows/changelog.yml` (PR changelog enforcement behavior)
- `src/getinp.f90` (`getinp` keyword toggles added in releases, for example connectivity options)
- `src/cli_parser.f90` (`parse_command`, `parse_command_line_args`; `-i -o` behavior)
- `testing/test_connectivity.sh` (connectivity-related regression harness)
- `testing/input_files/water_box_conect.inp` (existing connectivity-focused input sample)
- `testing/test_cli.sh` (CLI behavior regressions)
- Prefer targeted source search (for example: `rg -n "<version_or_keyword>" CHANGELOG.md app python src .github testing`).
