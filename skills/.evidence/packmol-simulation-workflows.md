# Evidence: packmol-simulation-workflows

## Primary docs
- `README.md`
- `testing/input_files/water_box.inp`
- `testing/input_files/mixture.inp`
- `testing/input_files/water_box_pbc.inp`
- `testing/input_files/bilayer_pbc.inp`

## Primary source entry points
- `app/packmol.f90`
- `src/setsizes.f90`
- `src/getinp.f90`
- `src/computef.f90`
- `src/pgencan.f90`
- `src/pbc.f90`
- `src/output.f90`
- `src/writesuccess.f90`
- `testing/runtests.jl`
- `testing/test_cli.sh`

## Extracted headings
- packmol: Simulation Workflows
- Route conditions
- Triage questions
- Canonical workflow
- Minimal working examples
- Convergence and validation checks

## Executable command hints
- `./configure && make`
- `cd testing && ../packmol < input_files/mixture.inp > mixture.log`
- `cd testing && julia runtests.jl -packmol ../packmol ./input_files/mixture.inp`

## Warnings and pitfalls
- Run bundled templates from `testing/` to resolve relative `structure_files` paths.
- Set fixed `seed` values for deterministic comparisons.
- Connectivity checks may require `testing/input_files/water_box_conect.inp`.
