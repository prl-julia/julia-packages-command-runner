# Run Commands Against Julia Packages in Batch

The packages are processed in parallel using `GNU parallel`.

The only documentation available at this point is comments in the Bash scripts.

- `proc_package_parallel.sh` entry point for the parallel processor. A very thin
  wrapper around the next one.

- `proc_package.sh` — does the actual job.
