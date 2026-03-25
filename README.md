This needs completing.

## some notes on model fitting and setup ##

NOTE: R 4.5.2 on macOS aarch64 (Apple Silicon) defaults to Apple's vecLib BLAS
which breaks mclapply-based parallelism used by EMC2.
To enable parallel fitting, switch to standard R BLAS in terminal:
  sudo ln -sf libRblas.0.dylib libRblas.dylib
(in /Library/Frameworks/R.framework/Resources/lib/)
To revert: sudo ln -sf libRblas.vecLib.dylib libRblas.dylib
Alternatively, run with cores_per_chain = 1 (slow but works without the fix)