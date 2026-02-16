---
name: packmol-build-and-install
description: This skill should be used when users ask about build and install in packmol; it prioritizes documentation references and then source inspection only for unresolved details.
---

# packmol: Build and Install

## High-Signal Playbook

### Route conditions
- Use this skill when the user asks how to install Packmol, compile it, package it, or verify the executable/CLI.
- Route input-keyword or modeling-method questions to `packmol-simulation-workflows`.
- Route release-history or upgrade-impact questions to `packmol-changelog`.

### Triage questions
- Which install path is required: prebuilt wheel (`pip`/`uvx`), local source build (`make`/`cmake`), or `fpm`? (`README.md`)
- Which OS/arch and Fortran compiler are available (`gfortran`, `ifort`, `ifx`, custom path)? (`README.md`, `configure`)
- Do they need a system `packmol` binary or the packaged Python CLI entry point? (`pyproject.toml`, `python/packmol/cli.py`, `python/hatch_build.py`)
- How is Packmol being invoked: stdin redirection, `-i`, or `-i -o`? (`README.md`, `src/cli_parser.f90`)
- Is deterministic output required (fixed `seed`) for diffs/regression checks? (`testing/test_cli.sh`)
- Are structure-file paths valid from the current working directory? (`testing/input_files/*.inp`, `src/setsizes.f90`)

### Canonical workflow
1. Pick distribution mode from `README.md`: wheel (`pip install packmol`), `uvx packmol`, local `make`, `cmake`, or `fpm`.
2. For local `make`, run `./configure [optional compiler path]` to set `FORTRAN=...` in `Makefile`.
3. Build Packmol (`make`, or `cmake ./ && cmake --build ./`, or `fpm install --profile release`).
4. Confirm executable availability (`./packmol` in repo build, or `packmol` in `PATH` for wheel/fpm installs).
5. Run smoke tests from `testing/` so relative paths in sample inputs resolve.
6. Validate CLI mode parity when needed (`stdin`, `-i`, `-i -o`) with `testing/test_cli.sh`.
7. Escalate to function-level source entry points when behavior is unclear.

### Minimal working example
- Local build + smoke test (`README.md`, `testing/input_files/water_box.inp`):
```bash
./configure
make

cd testing
../packmol -i input_files/water_box.inp -o output_files/water_box.smoke.pdb > water_box.smoke.log
rg -n "Success!" water_box.smoke.log
test -s output_files/water_box.smoke.pdb
```

- CLI invocations expected to be supported (`README.md`, `src/cli_parser.f90`, `testing/test_cli.sh`):
```bash
../packmol < input_files/water_box.inp
../packmol -i input_files/water_box.inp
../packmol -i input_files/water_box.inp -o output_files/water_cli.pdb
```

### Pitfalls and fixes
- `ERROR: Could not find any fortran compiler.`: install a supported compiler or pass an explicit compiler path to `./configure`. (`configure`)
- Command-line error with `-i/-o`: only `stdin`, `-i <inp>`, and `-i <inp> -o <out>` patterns are accepted; malformed flags fail fast. (`src/cli_parser.f90`)
- Same file used for both input and output: Packmol stops with a command-line error; use different paths. (`src/cli_parser.f90`)
- Nondeterministic CLI diffs with `seed -1`: set a fixed seed for reproducible regression outputs. (`testing/test_cli.sh`)
- Output/structure file not found: run from the directory expected by relative paths in the input file (for bundled templates, run from `testing/`). (`testing/input_files/*.inp`, `src/setsizes.f90`)
- Python wheel build fails to compile binary: ensure `gfortran` is discoverable and Fortran source tree is present. (`python/hatch_build.py`)

### Convergence and validation checks
- Build phase: executable exists (`test -x ./packmol`) and returns success on a smoke input.
- Packing quality: minimum inter-molecular distance should satisfy tolerance logic used in `testing/runtests.jl` (`mind.d >= (1 - precision) * tolerance`).
- Interface consistency: outputs from stdin vs `-i` vs `-i -o` should be equivalent for fixed-seed inputs. (`testing/test_cli.sh`)
- Failure-path sanity: known bad inputs should fail with expected markers (for example `FORCED`, `outside`). (`testing/test_failed.sh`)

## Scope
- Handle questions about build, installation, compilation, and environment setup.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `README.md`
- `CMakeLists.txt`

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
- `configure` (compiler search loop and `FORTRAN=` rewrite in `Makefile`)
- `Makefile` (`all`, `devel`, `perf`, `static` targets and compile/link flags)
- `CMakeLists.txt` (`add_executable(packmol ...)` and install destination)
- `fpm.toml` (`[[executable]]` definition and package version metadata)
- `python/hatch_build.py` (`CustomBuildHook.initialize`, `_find_gfortran`)
- `python/packmol/cli.py` (`get_binary_path`, `main`)
- `src/cli_parser.f90` (`parse_command`, `parse_command_line_args`)
- `src/setsizes.f90` (`setsizes`: file open and structure-file validation)
- `src/getinp.f90` (`getinp`: keyword parsing defaults used at runtime)
- `src/output.f90` (`output`, `write_connect`)
- `app/packmol.f90` (top-level call order from parse to output)
- `testing/test_cli.sh` (behavior equivalence expectations across invocation modes)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app python src .github testing`).
