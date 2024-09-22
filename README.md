# lcopt
(L)and (C)onservation Optimiser is an IP model for land conservation.
This project is a remake of an old hackathon project ([OPAL](https://github.com/leadnaut/opal)), which used a naive gradient descent algorithm and was extremely slow.
This version leverages Gurobi to solve a MIP model of the problem instead.

## MIP Model
### Sets
- $N$ - set of planning units (land plots)
- $E = \{(n,m) | n,m\in N\}$ - set of edges connecting plots.
- $K$ - set of planning features (species)
### Data:
- $c_n$ - cost of plot $n$
- $r_{nk}$ - contribution of planning feature $k$ at plot $n$.
- $T_k$ - target threshold of planning feature $k$ in an optimal solution.
- $v_{e}, \forall e\in E$ - bonus for selecting adjacent units of the edge (usually some measure of shared border length).
- $b$ - multiplier for adjacency bonuses.
- $R_n \subset N, \forall n \in N$ - set of plots that must also be selected if $n$ is selected.
### Variables:
- $x_n, \forall n\in N$ - 1 if plot $n$ is selected, 0 otherwise.
- $y_e, \forall e\in E$ - 1 if the plots connected by $e$ are both selected, 0 otherwise.
### Objective Function:
$$ \min \sum_{n\in N} c_n x_n - b\sum_{e\in E}v_e y_y$$
### Constraints:
- $y$ variables must correctly represent the status of their connected units.

$$2 y_e \leq x_n + x_m,\forall e =(n,m)\in E$$
- Target thresholds must be reached.

$$\sum_{n\in N} r_{nk} x_n \geq T_k, \forall k \in K $$
- Required units must be selected for a unit to be selected.

$$ \left|R_n\right|x_n \leq \sum_{m\in R_n} x_m , \forall n\in N$$
