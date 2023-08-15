## SOLVED 15/08/2023
Non-determinism being caused by computation time dependent rho as explained in [this issue](https://github.com/osqp/osqp-python/issues/116).
Current solution is to disable adaptive rho.

----


Demonstration of non-determinism in [cvxpy](https://www.cvxpy.org/index.html)'s quadratic programming.


## Background

[PROCESS](https://github.com/ukaea/PROCESS) is a systems code actively developed at the UK Atomic Authority which optimises fusion reactor designs. 

PROCESS uses [PyVMCON](https://github.com/ukaea/PyVMCON) as its optimiser. PyVMCON is a fully Python implementation of the [VMCON](https://cds.cern.ch/record/125407/files/CM-P00068640.pdf) optimisation algorithm.

## The problem

We are observing the solution to our `large-reactor` regression test 'flip-flopping' randomly between two converged solutions. 

We have traced this back to different solutions coming out of the quadratic programming problem solver. That is, for the same inputs the qsp non-deterministically converges on one of two optima. We have observed this happening on the same iteration of the optimiser (6th) consistently. The first five iterations never seem to deviate and their solutions *appear* deterministic.

We have recreated these conditions in `recreate.ipynb` to demonstrate the non-deterministic solutions. 

Both solutions *appear* to be valid--just different--and VMCON is able to converge on valid--just different--reactor configurations with both solution. 

1. Is this non-derminism expected? That is, are stochastic numerical methods used which could explain these two solutions?
2. Why are we only observing this non-determism in this extreme example? We have not been able to recreate this issue elsewhere.
3. Is there a way to eliminate this non-determinism for our test suite? Seeding the numpy random generator appears not to work.

## Technical details
- OS: 
    - Ubuntu 18.04.4 LTS (Linux 4.15.0-197-generic) (x86-64)
    - macOS 13.0.1 (22A400) (AArch64)
- CVXPY Version: 1.3.2
- OSQP Versions: 
    - 0.4.1
    - 0.6.1
    - 0.6.2
    - 0.6.3
    - 1.0.0b1
