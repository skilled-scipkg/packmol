# packmol source map: Changelog

Generated from source roots:
- `app`
- `python`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `changelog`
- `version`
- `release`
- `ignore_conect`
- `non_standard_conect`
- `-i`
- `-o`
- `command-line`
- `connect`
- `CONECT`
- `parse_command_line_args`
- `getinp`
- `title`
- `Skip-Changelog`

## Fast source navigation
- `rg -n "Version|ignore_conect|non_standard_conect|-i|-o|update_version.sh|update_version" CHANGELOG.md src update_version.sh`
- `rg -n "parse_command|parse_command_line_args|input_file_name|output_file_name" src/cli_parser.f90`
- `rg -n "subroutine getinp|ignore_conect|non_standard_conect|seed|precision" src/getinp.f90`
- `rg -n "subroutine title|Version " src/title.f90`
- `rg -n "version\\s*=|project\\.scripts|hatch_build|packmol.cli" fpm.toml pyproject.toml python`
- `rg -n "changelog|Skip-Changelog|changelog-enforcer" .github/workflows/changelog.yml .github/workflows`
- `rg -n "connect|conect|CONECT|cli" testing/test_connectivity.sh testing/test_cli.sh testing/input_files`

## Suggested source entry points
- `CHANGELOG.md` | source of truth for announced changes and version chronology
- `update_version.sh` | script that updates version strings and prints commit range hints
- `src/title.f90` | `title` subroutine runtime banner/version text source
- `fpm.toml` | Fortran package version tracking
- `pyproject.toml` | Python package version tracking and CLI entry point
- `.github/workflows/changelog.yml` | pull-request changelog enforcement gate
- `src/getinp.f90` | `getinp` keyword-level behavior changes referenced in release notes
- `src/cli_parser.f90` | `parse_command` and `parse_command_line_args` behavior (`-i`, `-o`, argument validation)
- `testing/test_connectivity.sh` | regression harness for connectivity semantics
- `testing/input_files/water_box_conect.inp` | existing connectivity-focused input example for manual checks
- `testing/test_cli.sh` | regression test for CLI invocation parity
- `.github/workflows/actions.yml` | CI compile/test matrix where behavior changes surface
