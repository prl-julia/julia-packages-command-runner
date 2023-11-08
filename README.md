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
  [56ddb016] Logging `@stdlib/Logging`
  [d6f4376e] Markdown `@stdlib/Markdown`
  [9a3f8284] Random `@stdlib/Random`
  [ea8e919c] SHA v0.7.0 `@stdlib/SHA`
  [9e88b42a] Serialization `@stdlib/Serialization`
  [8dfed614] Test `@stdlib/Test`
     Testing Running tests...
     Testing Multisets tests passed 

```
