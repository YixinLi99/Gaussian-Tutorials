# Gaussian-Tutorials
This tutorials aim to illustrate how we adjust our forcefield based on DFT methods on Gaussian, therefore, the forcefield will achieve more accuracy than the way simply imported from pdb file (see more at NAMD-Tutorial)

## Introduction to Gaussian Program
Gaussian is capable of predicting many properties of molecules and reactions, including the
following: 
- Molecular energies and structures 
- Energies and structures for transition states
- Bond and Reaction Energies
- Molecular Orbitals 
...
Computations can be carried out in gas phase or solutions, in ground states or excited states

### Gaussian Input Files 

## Use gaussian to generate a DFT method forcefield
## DFT calculations

There are many functionals: e.g. B3LYP
Hybird Functionals Pure Functionals 

#### How to choose which functional to use?
> https://gaussian.com/dft/


## Technically, to write a Gaussian conf file
### Commands in Gaussian to run: 
```
g16 job-name
g16 <input-file >output-file
```

### Shell scripts

Example：
```
#!/bin/sh

g16 <<END >water.log
%Chk=water
# RHF/6-31G(d)
 
water energy
 
0  1
O
H  1  1.0
H  1  1.0  2  120.0
 
END
echo "Job done."
