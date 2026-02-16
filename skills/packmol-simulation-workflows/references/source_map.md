# packmol source map: Simulation Workflows

Generated from source roots:
- `app`
- `src`
- `testing`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `tolerance`
- `structure`
- `inside`
- `outside`
- `pbc`
- `seed`
- `precision`
- `ignore_conect`
- `non_standard_conect`
- `Success!`
- `CONECT`

## Fast source navigation
- `rg -n "parse_command_line_args|setsizes|getinp|output|writesuccess" app/packmol.f90 src/cli_parser.f90 src/setsizes.f90 src/getinp.f90 src/output.f90 src/writesuccess.f90`
- `rg -n "tolerance|precision|seed|ignore_conect|non_standard_conect|pbc|structure|inside|outside" src/getinp.f90 testing/input_files/*.inp`
- `rg -n "subroutine output|subroutine write_connect|CRYST1|CONECT" src/output.f90 testing/test_connectivity.sh`
- `rg -n "check_mind|mind.d|tolerance" testing/runtests.jl`
- `rg -n "pgencan|packmolprecision|maxit|nloop|nloop0" src/pgencan.f90 src/getinp.f90`

## Suggested source entry points
- `app/packmol.f90` | main call flow (`parse_command_line_args` -> `setsizes` -> `getinp` -> optimization -> output)
- `src/cli_parser.f90` | `parse_command` and `parse_command_line_args` CLI contract
- `src/setsizes.f90` | `setsizes` input and structure file opening/validation
- `src/getinp.f90` | `getinp` keyword parsing and runtime defaults
- `src/computef.f90` | objective-function terms for distances and restraints
- `src/pgencan.f90` | `pgencan` and `packmolprecision` convergence behavior
- `src/pbc.f90` | periodic wrapping and distance primitives
- `src/output.f90` | `output` and `write_connect` output behavior
- `src/writesuccess.f90` | success criteria reporting
- `testing/runtests.jl` | `check_mind` minimum-distance validation logic
- `testing/test_cli.sh` | expected equivalence across stdin, `-i`, and `-i -o` execution modes
- `testing/test_connectivity.sh` | connectivity regression harness and input filename assumptions
