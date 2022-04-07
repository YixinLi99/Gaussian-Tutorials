# Forcefield Tutorial

This tutorial aim to (1) introduce some general forcefields, (2) how to choose the proper forcefield for the system;
and if the forcefield is not applicable to the system, (3) several methods to construct (e.g. DFT, Machine Learning (QuantumATK))

## Forcefield, $`E`$
```math
E_{total}=E_{bonded}+E_{non-bonded}
```
```math
E_{bonded}=\sum_{bonds} E^{b}+\sum_{angles} E^{a}+\sum_{dihedrals} E^{d}+\sum_{impropers} E^{i}
```
```math
E_{non-bonded}=\sum_{A>B}(E^{Coulombic Force}_{AB}+E^{van der Waals}_{AB})
```
Therefore,  
Class I:  
```math
E_{total}=\sum_{bonds} \frac{1}{2} k^{b}(r-r_0)^2 + \sum_{angles} \frac{1}{2} k^{a} (\theta-\theta_0)^2 + \sum_{dihedrals} \frac{V}{2} [1+cos(n\phi-\phi_s)] + \sum_{improper} \frac{1}{2} k^i (\xi-\xi_0)^2 + \sum_{A>B} [\frac{C_{12}}{r^{12}_{AB}}
-\frac{C_6}{r^6_{AB}}] + \sum_{A>B} \frac{q_A q_B}{4\pi\epsilon_0 r_AB}
```

## Some General Forcefield

#### AMBER Forcefield 


## Change itp.file with RESP 
This way it will optimise its non-bonded term performance 

## Fit its forcefield parameters
This way it will make sure the MD undergoes will have the same dihedral performance as QM  
More Compuational Details, see: "有机共轭小分子构建Gromacs程序使用的输入文件(包含单个二面⻆拟合)的方法.pdf"
<img width="6832" alt="有机共轭小分子构建Gromacs程序使用的输入文件(包含单个二面⻆拟合)的方法" src="https://user-images.githubusercontent.com/74641841/162131201-8e26de40-6c04-47b8-8763-05a76ea9d05a.png">


**step1: QM method**
Using GaussView to have a soft scanning of the molecule 
Then, constraint the dihedral angle 1,6,7,12 to rotate while optimise other parts 
it will generate the Free Energy (Hartree)
`struc1_scan.gjf` `struc1_scan.out`
`traj.trr` `traj.xyz`

**step2: MM method**

locate [ dihedraltypes ]:
remove the terms with 1,6,7,12; 1,6,7,9; 5,6,7,9; 5,6,7,12 and rerun to compare with QM data
therefore the difference will be the intrinsic energy 

**step3: Fitting**
then use the fitting codes to 
where improper dihedral angle (multiple) wiht the func 9 has the following equation:
```math
=k_{\phi}(1-cos(n\phi+\phi_{s}))  
```
using the `curve-fit` function with k1,k2,k3,k4 or k1,k2,k3,k4,k5,k6,k7,k8 to see whether it fit the dihedral parameters  
if k1,k2,k3,k4 is fit, then add it to the itp file  
since: *对于func=9的二面角项，对相同类型如此定义多次会起到累加效果*  
then simply put e.g. 1 6 7 12 k1 0  
                     1 6 7 12 k2 180  
                     1 6 7 12 k3 0  
                     1 6 7 12 k4 180  
                     
-OR-  
use Origin
描述化学键势能函数曲线最好的势函数之一是Morse Potential：
```math
E(r)=D[1-e^{-\alpha(r-r_0)}]^2
```
```math
\alpha=\sqrt{\frac{k}{2D}
```
