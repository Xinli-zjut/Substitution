# Numerical tests for a (singular) solution found by Smirnov

This directory contains the Julia code used for the numerical tests reported in Section 5.3 of the paper. The tests are performed on a solution in V(3,3,3|23) found by Smirnov.


## Supplied branches and final families

| branch | fixed set `I` | symbolic set `S` | verified gap | final family | parameter chart |
|---|---|---:|---:|---|---|
| small | `I603_gap2.txt` | `S18_gap2.txt` | 2 | `s722_t2.ipynb` | $t_1\ne0$, while $t_2$ may vanish; base point at $(-1,0)$ |
| large | `I550_gap15.txt` | `S71_gap15.txt` | 15 | `s722_t9.ipynb` | $t_1t_4t_5t_6t_7\ne0$; base point at $(-1,0,0,1,-1,1,1,0,0)$ |

The coordinate counts are 621 in total: `|I|+|S|=621` in both branches.


## File guide

| file | role |
|---|---|
| `s722.ipynb` | exact Smirnov decomposition `(U_k,V_k,W_k)`, `k=1,...,23` |
| `Ts.ipynb`, `Ts.jls` | construct/store the known de Groote and layer-scaling tangent basis matrix |
| `Ns.ipynb`, `Ns.jls` | construct the Brent Jacobian and store an exact nullspace basis matrix|
| `Findgap.ipynb` | modular randomized search for `I`, `S`, and a prescribed rank gap |
| `I603_gap2.txt`, `S18_gap2.txt` | the index set and its complement |
| `I550_gap15.txt`, `S71_gap15.txt` | the index set and its complement |
| `rankgapcomp.ipynb` | verify a supplied or newly generated `I` file |
| `StoXYZ_Oscar.ipynb` | convert row indices in `S` to Oscar variables |
| `Subsystemgen_Oscar.ipynb` | generate, simplify, split, and export reduced Brent subsystems |
| `Grobcomp_Oscar.ipynb` | compute the dimension and a Gröbner basis of one exported subsystem |
| `s722_t2.ipynb` | exact symbolic verification of the two-parameter family |
| `s722_t9.ipynb` | exact symbolic verification of the nine-parameter family |


### Computational steps

The computational steps are similar to those described in the previous directories.
