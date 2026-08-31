# Numerical tests for a (smooth) solution found by AlphaTensor

This directory contains the Julia code used for the numerical tests reported in Section 5.2.2 of the paper. The tests are performed on an AlphaTensor solution in V(4,4,4|49).

The workflow constructs the Jacobian nullspace and the known group tangent directions, searches for coordinate sets with a positive rank gap, builds reduced Brent systems, and derives parameterized solution families.

## Parameterized families included in this directory

Two precomputed coordinate selections are provided.

| Branch | Fixed-coordinate set | Symbolic-coordinate set | Final family |
|---|---|---|---|
| Rank gap 1 | `Igap1_size2344.txt`, $\lvert I\rvert=2344$ | `Sgap1_size8.txt`, $\lvert S\rvert=8$ | One-parameter family in [`s526_rankgap1_t.ipynb`](s526_rankgap1_t.ipynb) |
| Rank gap 10 | `Igap10_size2269.txt`, $\lvert I \rvert=2269$ | `Sgap10_size83.txt`, $\lvert S\rvert=83$ | Eight-dimensional family in [`s526_rankgap10_t.ipynb`](s526_rankgap10_t.ipynb) |



## Software requirements

The notebooks were prepared for Julia 1.12.x and Jupyter/IJulia. The principal external packages are:

```julia
using Pkg
Pkg.add([
    "IJulia",
    "NBInclude",
    "Nemo",
    "Oscar",
    "HomotopyContinuation",
    "MAT",
])
```

`LinearAlgebra`, `SparseArrays`, `Serialization`, `Random`, and `DelimitedFiles` are Julia standard libraries.

The `.jls` files use Julia's `Serialization` format. If they cannot be deserialized in a different Julia/package environment, regenerate them by running `Ts.ipynb` and `Ns.ipynb`.

## Recommended execution order

### A. Reproduce a parameter-family search

Run the common preliminary notebooks first:

```text
s526.ipynb
   ├── Ts.ipynb  ──> Ts.jls
   └── Ns.ipynb  ──> Ns.jls
                       ↓
                  Findgap.ipynb
                       ↓
                rankgapcomp.ipynb
```

Then choose either the rank-gap-1 or rank-gap-10 coordinate files and continue with:

```text
StoXYZ_Oscar.ipynb
        ↓
Subsystemgen_Oscar.ipynb
        ↓
Grobcomp_Oscar.ipynb
        ↓
manual component/parameter selection
        ↓
s526_rankgap1_t.ipynb
             or
s526_rankgap10_t.ipynb
```

The role of each step is as follows.

1. **`s526.ipynb`** loads the exact AlphaTensor solution.
2. **`Ts.ipynb`** constructs the tangent basis matrix $T(s)$ arising from the de Groote action and layer scaling, then saves it as `Ts.jls`.
3. **`Ns.ipynb`** constructs the Jacobian of the Brent equations, computes a basis matrix $N(s)$ for its nullspace, and saves it as `Ns.jls`.
4. **`Findgap.ipynb`** searches for an index set $I$ and its complement $S$ satisfying a prescribed rank-gap condition. Because the search is randomized, the two successful selections are stored in the supplied text files.
5. **`rankgapcomp.ipynb`** verifies the ranks and the resulting gap for the selected coordinate set.
6. **`StoXYZ_Oscar.ipynb`** converts the indices in $S$ into coordinate names `x[i,j,k]`, `y[i,j,k]`, and `z[i,j,k]`.
7. **`Subsystemgen_Oscar.ipynb`** substitutes the fixed coordinates indexed by $I$, constructs the reduced Brent system in the coordinates indexed by $S$, removes redundant equations, and separates independent subsystems.
8. **`Grobcomp_Oscar.ipynb`** computes the dimension and Gröbner basis of each generated subsystem. It must be run after the subsystem files have been generated.
9. The appropriate component and free parameters are selected from the Gröbner-basis output. This interpretation is currently a manual step.
10. The corresponding parameter-family notebook writes the matrices explicitly and verifies the $4\times4$ matrix-multiplication tensor identity.

### B. Verify the stored families only

To check the final families without repeating the rank-gap and Gröbner-basis computations, run either or both of:

```text
s526_rankgap1_t.ipynb
s526_rankgap10_t.ipynb
```

For the eight-dimensional family, the final verification first substitutes
`t7 = 1/t5 - t6` and then checks that every entry of the tensor difference is zero.

## File guide

| File | Purpose |
|---|---|
| `s526.ipynb` | Original AlphaTensor solution |
| `Ts.ipynb` | Construction of the known tangent space |
| `Ts.jls` | Serialized tangent basis matrix |
| `Ns.ipynb` | Jacobian and nullspace computation |
| `Ns.jls` | Serialized nullspace basis matrix |
| `Findgap.ipynb` | Randomized search for suitable sets $I$ and $S$ |
| `Igap1_size2344.txt`, `Sgap1_size8.txt` | the index set and its complement |
| `Igap10_size2269.txt`, `Sgap10_size83.txt` | the index set and its complement |
| `rankgapcomp.ipynb` | Rank-gap verification |
| `StoXYZ_Oscar.ipynb` | Conversion from row indices to Brent-variable names |
| `Subsystemgen_Oscar.ipynb` | Construction and decomposition of a reduced Brent system |
| `Grobcomp_Oscar.ipynb` | Gröbner-basis computation with Oscar.jl |
| `s526_rankgap1_t.ipynb` | Explicit one-parameter family |
| `s526_rankgap10_t.ipynb` | Explicit eight-dimensional family represented by nine symbols and one relation |

## Some notes

- A rank gap is a statement about first-order linearized data; it is not automatically the dimension of the nonlinear family obtained after solving the reduced system.
- The passage from a Gröbner basis to explicit parameter formulas includes component selection, free-parameter selection, and nonzero-denominator conditions. These steps are not fully automated in the present notebooks.
- Run the notebooks from this directory so that relative paths and `@nbinclude` statements resolve correctly.
- Several notebooks create or overwrite `.jls`, `.jl`, or `.txt` files. Keep a copy of the supplied data before rerunning the complete workflow.
