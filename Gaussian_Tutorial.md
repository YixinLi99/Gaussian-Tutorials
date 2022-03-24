# Gaussian-Tutorial
This tutorials aim to illustrate how we adjust our forcefield based on DFT methods on Gaussian, therefore, the forcefield will achieve more accuracy than the way simply imported from pdb file (see more at NAMD-Tutorial)

**Useful Resource:**  
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
whereas `Route section` → the job type (job type keywords), the method, the basic sets (stored internally + polarisation/diffuse functions)

## Use gaussian to generate a DFT method forcefield
## DFT calculations

There are many functionals: e.g. B3LYP, B3LYPD3, B3LYPD3(BJ)
Hybird Functionals Pure Functionals 
```# Opt B3LYP/6-31G* EmpiricalDispersion=GD3BJ```
> http://wild.life.nctu.edu.tw/~jsyu/compchem/g09/g09ur/k_dft.htm
> file:///Users/yixinli/Downloads/Grimme_pdf.pdf

#### How to choose which functional to use?
> https://gaussian.com/dft/
see also the tutorials provided by > http://bbs.keinsci.com/forum.php

## Technically, to write a Gaussian conf file
### Commands in Gaussian to run: 
```
g16 job-name
g16 <input-file> output-file
```
中间空格：`awk '{ print $3 $4 $5 }' AQx-2.mol2 | fgrep -w -v -f AQx-2.mol2 >out.txt`  
格式化输出：`awk '{printf("%-10s%s\n",$2,$3)}' AQx-2.mol2 | fgrep -w -v -f AQx-2.mol2 > out.txt`  
`{printf("%-10s%-10s%s\n",$2,$3,$4)}' AQx-2.mol2 | fgrep -w -v -f AQx-2.mol2 > out.txt` where as `%` denotes the $N of rows  

**Useful links**: > https://www.grymoire.com/Unix/Awk.html

### Shell scripts

Example：[Run.in]
```
%NProcShared=16
%Mem=32GB
%chk=AQx.chk
# Opt B3LYP/6-31G* EmpiricalDispersion=GD3BJ

AQx

0 1
C         -1.7350             -2.3350             0.4720
C         -0.3560             -2.2360             0.4640
```
where **X**:  
specifies the amount of detail you want to acquire: X = T (terse output);
X = P (max output); X = N (normal output)
where **functional**:  
where **basis sets**:  
6-31G: 6 GTOs for inner shell, 3 GTOs for inner valence, 1 GTO for outer valence  
6-31G* or 6-31G**: polarisation
by letting the AOs distort from original shape (get polarized under the influence of
the surroundings). Polarization can be added as * or (d).
- (d) or * type : d-type functions added on to atoms other than Hydrogens and
f-type functions added on to transition metals
- (d,p) or ** type : p-type functions added on to Hydrogens, d-type functions
added on to all other atoms, f-type functions added on to transition metals
> https://barrett-group.mcgill.ca/tutorials/Gaussian%20tutorial.pdf  

where **EmpiricalDispersion**:  
it is advised to use it at any occasions

therefore, -> `#P Opt B3LYP/6-31G** EmpiricalDispersion=GD3BJ`
```
END
echo "Job done."
```
### Job scripts
Example: [AQx_opt.sh]
```
#!/bin/bash

#SBATCH --nodes=1 --ntasks-per-node=4
#SBATCH --mem-per-cpu=2000
#SBATCH --time=0-1000:00:00
#SBATCH --output=slrum.stdout.txt
#SBATCH --error=slurm_stderr.txt
#SBATCH -p X20Cv4
#SBATCH -J GAUSSIAN

jobname=test

module load gaussian/g16.a03-avx

g16 Run.in
```
this will generate Run.log file and AQx.chk file  
if we would like to convert the output *chk.file* to *fchk.file*, try this: [AQx_opt_fchk.sh]
```                                                                                                                                        
#!/bin/sh

#SBATCH --nodes=1 --ntasks-per-node=4
#SBATCH --mem-per-cpu=2000
#SBATCH --time=0-1000:00:00
#SBATCH --output=slrum.stdout.txt
#SBATCH --error=slurm_stderr.txt
#SBATCH -p X28Cv4
#SBATCH -J GAUSSIAN

jobname=test

module load gaussian/g16.a03-avx2

formchk AQx.chk AQx.fchk

echo $PATH
``` 
the output *fchk.file* can be visualised via avogadro
