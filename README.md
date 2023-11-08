# Run Commands Against Julia Packages in Batch

Currently, the command to run is hardcoded to be `Pkg.test("<package>")`.

The packages are processed in parallel using `GNU parallel`.

The only documentation available at this point is comments in the Bash scripts.

- `proc_package_parallel.sh` entry point for the parallel processor. A very thin
  wrapper around the next one.

- `proc_package.sh` — does the actual job.

## Example run on one package

Should work in any directory, but for simplicity we start from the root of the
repo and create a `scratch` subdir:

``` shellsession
❯ mkdir scratch && cd scratch
❯ BATCH=true ../proc_package.sh "Multisets"
❯ exa -T -L 2
.
└── Multisets
   ├── depot
   ├── test-out.txt
   └── test-result.txt

❯ cat Multisets/test-out.txt
    Updating registry at `.../scratch/Multisets/depot/registries/General.toml`
   Resolving package versions...
  No Changes to `.../scratch/Multisets/depot/environments/v1.8/Project.toml`
  No Changes to `.../scratch/Multisets/depot/environments/v1.8/Manifest.toml`
     Testing Multisets
      Status `/tmp/jl_XB3SXQ/Project.toml`
  [3b2b4ff1] Multisets v0.4.4
  [8dfed614] Test `@stdlib/Test`
      Status `/tmp/jl_XB3SXQ/Manifest.toml`
  [3b2b4ff1] Multisets v0.4.4
  [2a0f44e3] Base64 `@stdlib/Base64`
  [b77e0a4c] InteractiveUtils `@stdlib/InteractiveUtils`
  ...
     Testing Running tests...
     Testing Multisets tests passed 

```

# Example run on list of packages

``` shellsession
❯ BATCH=true ../proc_package_parallel.sh ../pkg-list-sample.txt

❯ exa -T -L 2
.
├── MethodAnalysis
│  ├── depot
│  ├── test-out.txt
│  └── test-result.txt
└── Multisets
   ├── depot
   ├── test-out.txt
   └── test-result.txt

❯ cat MethodAnalysis/test-out.txt
    Updating registry at `/mnt/data/artem/Code/julia-projects/julia-packages-test-runner/scratch/MethodAnalysis/depot/registries/General.toml`
   Resolving package versions...
  No Changes to `/mnt/data/artem/Code/julia-projects/julia-packages-test-runner/scratch/MethodAnalysis/depot/environments/v1.8/Project.toml`
  No Changes to `/mnt/data/artem/Code/julia-projects/julia-packages-test-runner/scratch/MethodAnalysis/depot/environments/v1.8/Manifest.toml`
     Testing MethodAnalysis
      Status `/tmp/jl_43pbeJ/Project.toml`
  [1520ce14] AbstractTrees v0.4.4
  [a09fc81d] ImageCore v0.10.1
  [85b6ec6f] MethodAnalysis v0.4.13
  ...

Test Summary:            | Pass  Total  Time
methodinstances_owned_by |    6      6  0.6s
Test Summary: | Pass  Total  Time
Backedges     |   26     26  0.4s
Test Summary: | Pass  Total  Time
call_type     |    2      2  0.0s
Test Summary: | Pass  Total  Time
Invalidation  |    3      3  0.1s
Test Summary: | Pass  Total  Time
hasbox        |    2      2  0.1s
Test Summary: | Pass  Broken  Total  Time
findcallers   |   20       2     22  2.7s
     Testing MethodAnalysis tests passed 
```
