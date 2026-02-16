---
name: packmol-simulation-workflows
description: This skill should be used when users ask how to prepare Packmol inputs, execute packings, and validate outputs for real simulation startup.
---

# packmol: Simulation Workflows

## High-Signal Playbook

### Route conditions
- Use this skill for preparing `.inp` files, selecting templates, running Packmol, and validating outputs before molecular simulations.
- Route compiler/packaging questions to `packmol-build-and-install`.
- Route release-history and changelog policy questions to `packmol-changelog`.

### Triage questions
- Which template is closest to the target system (`water_box`, `mixture`, `bilayer`, `spherical`, `solvprotein`)?
- Are periodic boundaries required (`*_pbc.inp`)?
- Is deterministic output required (fixed `seed` instead of `seed -1`)?
- Are connectivity lines required (`ignore_conect`, `non_standard_conect`)?
- Is the workflow using stdin mode, `-i`, or `-i -o` invocation?

### Canonical workflow
1. Build or locate a Packmol binary (`./packmol` from repository root or `../packmol` from `testing/`).
2. Start from the closest template in `testing/input_files`.
3. Run from `testing/` so relative template paths (`./structure_files/...`) resolve.
4. Validate success markers and output-file generation.
5. Run quality checks (`testing/runtests.jl`) when available for minimum-distance validation.
6. Compare stdin and CLI invocations for pipeline integrations.
7. Escalate to `references/source_map.md` for function-level debugging when output is unexpected.

### Minimal working examples
```bash
# from repository root
./configure
make

cd testing
../packmol < input_files/mixture.inp > mixture.log
rg -n "Success!" mixture.log
test -s output.pdb
mv -f output.pdb output_files/mixture.quickstart.pdb
```

```bash
# deterministic CLI parity check
cd testing
../packmol < input_files/water_box.inp > stdin.log
../packmol -i input_files/water_box.inp -o output_files/water_box.cli.pdb > cli.log
rg -n "Success!" stdin.log cli.log
test -s output.pdb
test -s output_files/water_box.cli.pdb
```

```bash
# optional quality check with Julia tooling
cd testing
julia runtests.jl -packmol ../packmol ./input_files/mixture.inp ./input_files/water_box_pbc.inp
```

### Pitfalls and fixes
- Running templates outside `testing/` breaks relative structure paths; run from `testing/` or rewrite paths.
- `seed -1` makes results time-dependent; set a fixed seed for reproducible workflows.
- Input keyword typos fail hard in `getinp`; verify with existing templates first.
- Connectivity regression script may fail on filename mismatch; use `testing/input_files/water_box_conect.inp` for manual checks.
- Reusing the same file for input and output triggers command-line validation errors.

### Convergence and validation checks
- Log contains `Success!` and no `ERROR:` lines.
- Output file exists and is non-empty.
- Minimum-distance checks satisfy tolerance via `testing/runtests.jl`.
- Stdin and `-i/-o` outputs are consistent for fixed-seed inputs.

## Scope
- Handle simulation startup workflows: input preparation, command execution, and output validation.
- Keep guidance operational: concrete commands, concrete inputs, and explicit checkpoints.

## Primary documentation references
- `README.md`
- `testing/input_files`

## Workflow
- Start with `references/doc_map.md`.
- Reuse existing input templates before authoring new files.
- Use test scripts as validation checkpoints.
- If behavior is unclear, inspect `references/source_map.md` and jump to listed routines.
- Cite exact file paths in responses.

## Tutorials and examples
- `testing/input_files`

## Test references
- `testing/runtests.jl`
- `testing/test_cli.sh`
- `testing/test_failed.sh`
- `testing/test_connectivity.sh`

## Optional deeper inspection
- `app`
- `src`
- `testing`

## Source entry points for unresolved issues
- `app/packmol.f90` (main execution flow from CLI parse to packing/output)
- `src/setsizes.f90` (`setsizes`: input and structure file opening/validation)
- `src/getinp.f90` (`getinp`: keyword parsing, defaults, and constraints)
- `src/computef.f90` (distance and restraint objective evaluation)
- `src/pgencan.f90` (`pgencan`, `packmolprecision`: optimization and convergence behavior)
- `src/pbc.f90` (`v_in_box`, `delta_vector`, `cell_ind`: periodic boundary logic)
- `src/output.f90` (`output`, `write_connect`: output formatting and CONECT handling)
- `src/writesuccess.f90` (`writesuccess`: final quality/success reporting)
- Prefer targeted source search (for example: `rg -n "<keyword_or_routine>" app src testing`).
