# Benchmark with multiple rho variables for time-varying specification satisfaction

## Basic run
### Requirements
 - Python `3.10` (or lower)
 - Required Packages: `numpy`, `treelib`, `gurobi`, `matplotlib`, `scipy`. For `conda`, they can be installed using the following commands:
```
conda install -c anaconda numpy
conda install -c conda-forge treelib
conda install -c gurobi gurobi
conda install -c conda-forge matplotlib
conda install -c anaconda scipy
```

### Toolbox stlpy

This benchmark is based on the `stlpy` toolbox (https://github.com/vincekurtz/stlpy/blob/main/README.md). Please cite the source when you develop your own benchmark.

### Instructions

- Run `main.py` to generate system trajectories;
- Run `plot_results.py` to plot the results.

### License

For the license of the `stlpy` toolbox, refer to `stlpy/LICENSE`.

## Changes made to include multiple rho variables

- Change the dimension of rho variables:
'''
self.rho = self.model.addMVar((self.T, ), name="rho", lb=0.0)   # 1->self.T to incorporate multiple dimensions
'''
- Change the cost function:
'''
def AddRobustnessCost(self):
        self.cost -= np.ones(self.rho.shape).T @ self.rho  # The cost is the negative summary of multiple rho values
'''
- Additional constraints to specify the satisfaction of specifications
'''
def AddRobustnessConstraint(self, rho_min=0.0):
        for rho in self.rho:
            self.model.addConstr( rho >= rho_min )
'''
-
```
if isinstance(formula, LinearPredicate):
            # self.rho -> self.rho[t]
            self.model.addConstr( formula.a.T@self.y[:,t] - formula.b + (1-z)*self.M  >= self.rho[t] )
```
