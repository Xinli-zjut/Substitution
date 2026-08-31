# Numerical tests for a (singular) solution found by AlphaTensor

This directory contains the Julia code used for the numerical tests reported in Section 5.3 of the paper. The tests are performed on a solution in V(4,4,4|49) found by AlphaTensor.


The directory supports the complete path from the singular base solution to rank-gap searches, reduced Brent subsystems, Gröbner bases, and explicit parameterized families.

## Supplied branches and final families

| branch | fixed set `I` | symbolic set `S` | verified gap | final family | parameter chart |
|---|---|---:|---:|---|---|
| small | `I2330_gap2.txt` | `S22_gap2.txt` | 2 | `s721_t2.ipynb` | two parameters, $t_1t_2\ne0$; base point at $(-1,-1)$ |
| large | `I2168_gap16.txt` | `S184_gap16.txt` | 16 | `s721_t12.ipynb` | twelve-parameter rational chart; base point at $(-1,0,0,1,-1,-1,-1,0,-1,1,1,0)$ |

The coordinate counts are 2352 in total: `|I|+|S|=2352` in both branches.




## File guide

| file | role |
|---|---|
| `s721.ipynb` | exact AlphaTensor decomposition `(U_k,V_k,W_k)`, `k=1,...,49` |
| `Ts.ipynb`, `Ts.jls` | construct/store the known de Groote and layer-scaling tangent basis matrix|
| `Ns.ipynb`, `Ns.jls` | construct the Brent Jacobian and store an exact nullspace basis matrix |
| `Findgap.ipynb` | modular randomized search for `I`, `S`, and a prescribed rank gap |
| `I2330_gap2.txt`, `S22_gap2.txt` | the index set and its complement  |
| `I2168_gap16.txt`, `S184_gap16.txt` | the index set and its complement|
| `rankgapcomp.ipynb` | verify a supplied or newly generated `I` file |
| `StoXYZ_Oscar.ipynb` | convert row indices in `S` to Oscar variables |
| `Subsystemgen_Oscar.ipynb` | generate, simplify, split, and export reduced Brent subsystems |
| `Grobcomp_Oscar.ipynb` | compute the dimension and a Gröbner basis of one exported subsystem |
| `s721_t2.ipynb` | exact symbolic verification of the two-parameter family |
| `s721_t12.ipynb` | exact symbolic verification of the twelve-parameter family |

### Computational steps

The computational steps are similar to those described in the previous directories.
