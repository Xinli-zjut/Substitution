# Numerical tests for the DPS solution

This directory contains the Julia code used for the numerical tests reported in Section 5.2.1 of the paper. The solution belongs to $\mathcal{V}(4,4,4\mid 48)$. It was found by Dumas, Pernet, and Sedoglavic.

The main computational objective is to fix a large set of fixed partial solution, construct the resulting reduced Brent system, compute its Gröbner basis, and obtain a parameterized solution family through the DPS solution.

## Main results stored in this directory

- `LRP.ipynb` stores the original exact rank-48 decomposition.
- `I_LRPt.txt` and `S_LRPt.txt` store a complementary pair of row-index sets:
  $|I|=2082$, $|S|=222$, and $I\cup S=[2304]$.
- `groebner_basis_LRPt.txt` stores the Gröbner-basis output used to derive the parameterized family.
- `LRP_t.ipynb` records and verifies a one-parameter family. Its formulas contain denominators involving `t`, so the displayed affine chart requires `t != 0`.
- `Invariant_LRPt.ipynb` computes invariants associated with the parameterized family.

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

The `.jls` files use Julia's `Serialization` format. If they cannot be deserialized in a different Julia/package environment, regenerate them by running `Ts-LRP.ipynb` and `Ns-LRP.ipynb`.

## Recommended execution order

### A. Reproduce the search for the parameterized family

Run the notebooks in the following order:

```text
LRP.ipynb
   ├── Ts-LRP.ipynb  ──> Ts.jls
   └── Ns-LRP.ipynb  ──> Ns.jls
                         ↓
                    Findgap.ipynb
                         ↓
                  rankgapcomp.ipynb
                         ↓
                   StoXYZOscar.ipynb
                         ↓
                Subsystemgen_Oscar.ipynb
                         ↓
                   Grobcomp_Oscar.ipynb
                         ↓
       manual component/parameter selection
                         ↓
                     LRP_t.ipynb
                         ↓
                Invariant_LRPt.ipynb
```

The role of each step is as follows.

1. **`LRP.ipynb`** loads the exact decomposition as 48 triples $(U_k,V_k,W_k)$.
2. **`Ts-LRP.ipynb`** constructs the tangent basis matrix $T(s)$ coming from the de Groote action and the layer-scaling action, then saves it as `Ts.jls`.
3. **`Ns-LRP.ipynb`** constructs the Jacobian of the Brent equations at the DPS solution, computes a basis matrix $N(s)$ for its nullspace, and saves it as `Ns.jls`.
4. **`Findgap.ipynb`** searches for a row-index set $I$ and its complement $S$ satisfying the prescribed rank-gap condition. This is a randomized search, so the precomputed files `I_LRPt.txt` and `S_LRPt.txt` are provided for reproducibility.
5. **`rankgapcomp.ipynb`** independently checks the ranks associated with the selected sets.
6. **`StoXYZOscar.ipynb`** converts the row indices in $S$ into coordinate names such as `x[i,j,k]`, `y[i,j,k]`, and `z[i,j,k]`. The resulting variable list is stored in `vars_S_LRPt.txt`.
7. **`Subsystemgen_Oscar.ipynb`** fixes the coordinates indexed by $I$, constructs the reduced Brent system in the coordinates indexed by $S$, removes redundant equations, and separates independent subsystems.
8. **`Grobcomp_Oscar.ipynb`** computes the dimension and Gröbner basis of the generated subsystem. It must be run after the subsystem files have been generated.
9. The relevant component and free parameter are then selected from the Gröbner-basis output. This algebraic interpretation is currently a manual step.
10. **`LRP_t.ipynb`** writes the resulting family explicitly and reconstructs the $4\times4$ matrix-multiplication tensor. The final verification should return zero.
11. **`Invariant_LRPt.ipynb`** studies invariant quantities along the family.

### B. Verify the already constructed family only

To check the final result without repeating the rank-gap and Gröbner-basis computations, run:

```text
LRP_t.ipynb
Invariant_LRPt.ipynb
```

## File guide

| File | Purpose |
|---|---|
| `LRP.ipynb` | Original DPS rank-48 decomposition |
| `Ts-LRP.ipynb` | Construction of the known tangent directions |
| `Ts.jls` | Serialized tangent-direction matrix |
| `Ns-LRP.ipynb` | Jacobian and nullspace computation |
| `Ns.jls` | Serialized nullspace data |
| `Findgap.ipynb` | Randomized search for suitable sets $I$ and $S$ |
| `I_LRPt.txt`, `S_LRPt.txt` | the index set and its complement |
| `rankgapcomp.ipynb` | Rank-gap verification |
| `StoXYZOscar.ipynb` | Conversion from row indices to Brent-variable names |
| `vars_S_LRPt.txt` | Variables that remain symbolic after substitution |
| `Subsystemgen_Oscar.ipynb` | Construction and decomposition of the reduced Brent system |
| `Grobcomp_Oscar.ipynb` | Gröbner-basis computation with Oscar.jl |
| `groebner_basis_LRPt.txt` | Saved Gröbner-basis output |
| `LRP_t.ipynb` | Explicit one-parameter solution family and tensor verification |
| `Invariant_LRPt.ipynb` | Invariant computations for the family |

## Important notes

- The **rank gap is first-order linear information** obtained from the Jacobian and the known group tangent directions. It should not automatically be identified with the dimension of the final nonlinear solution family.
- The passage from the Gröbner basis to the final parameter formulas includes component selection, the choice of free parameters, and nonzero-denominator conditions. These steps are not fully automated in the present notebooks.
- Run the notebooks from this directory so that relative file paths and `@nbinclude` statements resolve correctly.
- Several notebooks create or overwrite `.jls`, `.jl`, or `.txt` files. Keep a copy of the precomputed files before rerunning the complete workflow.
