# packmol source map: Build and Install

Generated from source roots:
- `app`
- `python`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `build`
- `compile`
- `configure`
- `make`
- `cmake`
- `fpm`
- `wheel`
- `cli`
- `-i`
- `-o`
- `input_file_name`
- `output_file_name`
- `parse_command_line_args`
- `setsizes`
- `getinp`
- `initialize`
- `get_binary_path`

## Fast source navigation
- `rg -n "build|compile|configure|FORTRAN|cibuildwheel" Makefile CMakeLists.txt python/hatch_build.py pyproject.toml .github/workflows`
- `rg -n "parse_command|parse_command_line_args|input_file_name|output_file_name" src/cli_parser.f90 app/packmol.f90`
- `rg -n "subroutine setsizes|open\\(|file-open error|structure" src/setsizes.f90`
- `rg -n "subroutine getinp|seed|precision|ignore_conect|non_standard_conect|pbc" src/getinp.f90`
- `rg -n "def initialize|def _find_gfortran|def get_binary_path|def main" python/hatch_build.py python/packmol/cli.py`
- `rg -n "subroutine output|subroutine write_connect|CRYST1|CONECT" src/output.f90 testing/test_cli.sh`

## Suggested source entry points
- `configure` | compiler auto-discovery loop and Makefile `FORTRAN=` rewriting
- `Makefile` | local build and cleanup targets (`all`, `devel`, `perf`, `static`, `clean`)
- `CMakeLists.txt` | CMake build graph and `install(TARGETS packmol DESTINATION bin)`
- `fpm.toml` | Fortran package metadata and executable declaration
- `pyproject.toml` | Python entry-point wiring (`project.scripts`) and custom build hook
- `python/hatch_build.py` | `CustomBuildHook.initialize` and `_find_gfortran`
- `python/packmol/cli.py` | `get_binary_path` and `main` runtime dispatch behavior
- `app/packmol.f90` | top-level call order (`parse_command_line_args` -> `setsizes` -> `getinp` -> output path)
- `src/cli_parser.f90` | `parse_command` and `parse_command_line_args` argument contract
- `src/setsizes.f90` | `setsizes` input/structure file open handling
- `src/getinp.f90` | `getinp` keyword defaults and runtime options
- `src/output.f90` | `output` and `write_connect` output-file behavior
- `.github/workflows/actions.yml` | CI compile matrix (`make` + `cmake`) and test invocation
- `.github/workflows/build-wheels.yaml` | wheel build/upload release pipeline
- `testing/test_cli.sh` | expected parity across stdin, `-i`, and `-i -o` invocation styles
