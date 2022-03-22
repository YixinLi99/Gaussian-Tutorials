# Gaussian-Tutorial
This tutorials aim to illustrate how we adjust our forcefield based on DFT methods on Gaussian, therefore, the forcefield will achieve more accuracy than the way simply imported from pdb file (see more at NAMD-Tutorial)

useful resources: 
General Introduction: > https://barrett-group.mcgill.ca/tutorials/Gaussian%20tutorial.pdf 
GaussianLab: Hand-on labs on how to use Gaussian program: > https://www.tau.ac.il/~ephraim/complab.html  
The Absolute Beginner Guide for Gaussian: > http://www.ccl.net/cca/documents/dyoung/topics-orig/gaussian.html
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
- `Link 0 Commands`: Locate and name scratch files. 
- `Route section (# lines)`: Specify desired calculation type, the method
and basis set and other options.
- `Title section`: Brief description of the calculation.
- `Molecule specification`: Specify molecular system to be studied in
Cartesian coordinates or by the Z-matrix.
- `Optional additional sections`: Additional input needed for specific job
types. 
whereas `Route section` -> the job type (job type keywords), the method, the basic sets (stored internally + polarisation/diffuse functions)
w
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
中间空格：`awk '{ print $3 $4 $5 }' AQx-2.mol2 | fgrep -w -v -f AQx-2.mol2 >out.txt` 
格式化输出：`awk '{printf("%-10s%s\n",$2,$3)}' AQx-2.mol2 | fgrep -w -v -f AQx-2.mol2 > out.txt`
`{printf("%-10s%-10s%s\n",$2,$3,$4)}' AQx-2.mol2 | fgrep -w -v -f AQx-2.mol2 > out.txt` where as '%' denotes the $N of rows

useful links: > https://www.grymoire.com/Unix/Awk.html

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
