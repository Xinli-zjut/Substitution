# Numerical tests for a (smooth) solution found by Laderman

This directory contains the Julia code used for the numerical tests reported in Section 5.2.3 of the paper. The tests are performed on a solution in V(3,3,3|23) found by Laderman.

The directory supports the complete path from the base solution to rank-gap searches, reduced Brent subsystems, Gröbner bases, and explicit parameterized families.

## Supplied branches and final families

| branch | fixed set `I` | symbolic set `S` | verified gap | final family | parameter chart |
|---|---|---:|---:|---|---|
| small | `Igap1_size613.txt` | `Sgap1_size8.txt` | 1 | `lad_t.ipynb` | one parameter, $t\ne0$; base point at $t=1$ |
| large | `Igap6_size574.txt` | `Sgap6_size47.txt` | 6 | `lad_t6.ipynb` | six parameters, $t_1\cdots t_6\ne0$; base point at $(-1,-1,1,1,1,1)$ |

The coordinate counts are 621 in total: `|I|+|S|=621` in both branches.

## Software requirements

The notebooks were saved with **Julia 1.12.6** and the `Julia 1.12` Jupyter kernel. Julia 1.12.x is therefore recommended, especially when reading the included serialized `.jls` files.


## File guide

| file | role |
|---|---|
| `lad.ipynb` | exact Laderman decomposition `(U_k,V_k,W_k)`, `k=1,...,23` |
| `Ts.ipynb`, `Ts.jls` | construct/store the known de Groote and layer-scaling tangent basis matrix |
| `Ns.ipynb`, `Ns.jls` | construct the Brent Jacobian and store an exact nullspace basis matrix |
| `Findgap.ipynb` | modular randomized search for `I`, `S`, and a prescribed rank gap |
| `Igap1_size613.txt`, `Sgap1_size8.txt` | the index set and its complement |
| `Igap6_size574.txt`, `Sgap6_size47.txt` | the index set and its complement |
| `rankgapcomp.ipynb` | verify a supplied or newly generated `I` file |
| `StoXYZ_Oscar.ipynb` | convert row indices in `S` to `x[i,j,k]`, `y[i,j,k]`, `z[i,j,k]` and generate an Oscar variable file |
| `Subsystemgen_Oscar.ipynb` | substitute fixed coordinates, simplify the Brent system, split it into independent subsystems, and export them |
| `Grobcomp_Oscar.ipynb` | compute the dimension and a Gröbner basis of one exported subsystem |
| `lad_t.ipynb` | exact symbolic verification of the one-parameter family |
| `lad_t6.ipynb` | exact symbolic verification of the six-parameter family |

## Two ways to use the directory

### A. Quick verification of the supplied results

1. To verify a supplied rank gap, open `rankgapcomp.ipynb`, set `I_FILE` to the desired supplied `I` file, and run all cells. The included `Ns.jls` and `Ts.jls` let you skip the expensive Jacobian/nullspace computation.
2. To verify an explicit parameterized family, run the corresponding family notebook directly. Its final cells construct the matrix-multiplication tensor and assert symbolically that every Brent equation vanishes.

### B. Full workflow for finding a parameterized family

The intended order is

```text
base-solution notebook
        |---> Ts.ipynb ---> Ts.jls
        `---> Ns.ipynb ---> Ns.jls
                         |
                         v
                   Findgap.ipynb
                         |
                         v
                 choose/verify I and S
                         |
                         v
                 StoXYZ_Oscar.ipynb
                         |
                         v
              Subsystemgen_Oscar.ipynb
                         |
                         v
                 Grobcomp_Oscar.ipynb
                         |
                         v
          extract formulas from the Gröbner basis
                         |
                         v
              parameter-family notebook
```

The last step—choosing free parameters and converting the Gröbner-basis relations into explicit matrix formulas—is currently **manual**. The supplied family notebooks record the resulting formulas and provide exact symbolic verification.



## Reproducibility notes

- `I` denotes the coordinates fixed at their base-solution values; `S` is its complement and contains the coordinates left symbolic. All indices are **1-based** and follow the layerwise order

  ```text
  [vec(U_1); vec(V_1); vec(W_1); ...; vec(U_r); vec(V_r); vec(W_r)].
  ```

  Julia uses column-major `vec` ordering.
- A rank gap is a linearized signal of possible directions beyond the known group action. It is not automatically the dimension of the final nonlinear parameterized family.
- `rankgapcomp.ipynb` is a convenient floating-point sanity check. For the stronger reproducibility check, use the multi-prime modular ranks and `verify_set` logic in `Findgap.ipynb`.
- `vars_..._oscar.jl`, `Brent_unique_subsystem_*.jl`, and `groebner_basis_*.txt` are generated intermediate files. They are intentionally not required for quick verification of the supplied final families.
- The included `.jls` files are Julia `Serialization` files. If `deserialize` fails in a different Julia/package environment, regenerate them by running `Ts.ipynb` and `Ns.ipynb` with Julia 1.12.x.
- On Windows, Oscar is best run in WSL. Clone and run the repository inside the WSL Linux filesystem (for example, under `~/Substitution`) rather than a Windows-mounted path.

