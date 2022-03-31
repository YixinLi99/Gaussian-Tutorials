# Forcefield Tutorial

This tutorial aim to (1) introduce some general forcefields, (2) how to choose the proper forcefield for the system;
and if the forcefield is not applicable to the system, (3) several methods to construct (e.g. DFT, Machine Learning (QuantumATK))

## Forcefield, E
```math
E{total}=E{bonded}+E{non-bonded}
E{bonded}=E{bonds}+E{angles}+E{dihedrals}+E{impropers}

```


## Some General Forcefield

#### AMBER Forcefield 


## Change itp.file with RESP 
This way it will optimise its non-bonded term performance 

## Fit its forcefield parameters
This way it will make sure the MD undergoes will have the same dihedral performance as QM

step1: QM method 
Using GaussView to have a soft scanning of the molecule 
Then, constraint the dihedral angle 1,6,7,12 to rotate while optimise other parts 
it will generate the Free Energy (Hartree)
`struc1_scan.gjf` `struc1_scan.out`
`traj.trr` `traj.xyz`

step2: MM method


remove the terms with 1,6,7,12; 1,6,7,9; 5,6,7,9; 5,6,7,12 to compare with QM data
therefore the difference will be the intrinsic energy 

then use the fitting codes to 
where improper dihedral angle (multiple) wiht the func 9 has the following equation:
=k_{\phi}(1-cos(n\phi+\phi_{s})
using the `curve-fit` function with k1,k2,k3,k4 or k1,k2,k3,k4,k5,k6,k7,k8 
to see whether it fit the dihedral parameters
