# QE-Tutorial

###### Step1: Install qe-6.7; Install gfortran and C++ at HPC; Install openmpi:
(gfortran and C++ are essential to run QE codes, if you've installed it already ignore it)
```
(base) Yixins-MacBook-Pro-2:~ yixinli$ cd Downloads

(base) Yixins-MacBook-Pro-2:Downloads yixinli$ gunzip gcc-10.2-bin.tar.gz

(base) Yixins-MacBook-Pro-2:Downloads yixinli$ sudo tar -xvf gcc-10.2-bin.tar -C /
```
you can test it by entering ```which gcc```

then, download openmpi-4.0.5 from web
```
(base) Yixins-MacBook-Pro-2:~ yixinli$ cd Downloads

(base) Yixins-MacBook-Pro-2:Downloads yixinli$ cd openmpi-4.0.5

(base) Yixins-MacBook-Pro-2:openmpi-4.0.5 yixinli$ make all install
```
some errors may occur during the installation, try to install x-code on your mac first
```
(base) Yixins-MacBook-Pro-2:~ yixinli$ cd QE

(base) Yixins-MacBook-Pro-2:QE yixinli$ cd qe-6.7

(base) Yixins-MacBook-Pro-2:qe-6.7 yixinli$ ./configure #if everything work well, it shows 'configuration success'

(base) Yixins-MacBook-Pro-2:qe-6.2 yixinli$ make all #there may be some error even if 'configuration success', try with more updated version of QE
```


###### Step2: Test
```
(base) Yixins-MacBook-Pro-2:QE yixinli$ cd qe-6.7

(base) Yixins-MacBook-Pro-2:qe-6.7 yixinli$ ./configure

(base) Yixins-MacBook-Pro-2:qe-6.7 yixinli$ which gfortran
/usr/local/bin/gfortran

(base) Yixins-MacBook-Pro-2:qe-6.7 yixinli$ which mpirun
```

###### Step 3: Run the scf file as following command
```
(base) Yixins-MacBook-Pro-2:qe-6.7 yixinli$ cd PW

(base) Yixins-MacBook-Pro-2:PW yixinli$ cd examples

(base) Yixins-MacBook-Pro-2:examples yixinli$ cd example01

(base) Yixins-MacBook-Pro-2:example01 yixinli$ mpirun -np 4 ~/QE/qe-6.7/bin/pw.x < si.band.input > si.band.output

curl -o C.UPF https://www.quantum-espresso.org/upf_files/C.pz-rrkjus.UPF
```
or 
```
mpirun -np 2 ~/QE/qe-6.7/bin/pw.x inp si.vc_relax > si.vc_relax.out
```
 > Reference: https://www.youtube.com/watch?v=uWhZOwfmV2s&ab_channel=PhysWhiz

Basically, this means we've completed our installations :)

----------------------------------------------------------------

First, Let's start calculations on band energies:

###### Transfer VASP file into scf file 

[transfer](http://www.densityflow.com/p2p.php)

if you don't have VASP file, you can search it for cif, and then transfer into QE input file
[理论](https://www.bilibili.com/video/av32743444)
in order to do that, you need to download cif2cell on your computer which requires python environment and python package: PyCfRW

###### Step1: vc-relax (结构优化）
get the structure 
change into vc-relax calcualtions:
```
&CONTROL
  calculation='vc-relax', 
  restart_mode='from_scratch'
  pseudo_dir='../pseudo/', 
  prefix='C2H4'
  outdir='../tmp',  
  forc_conv_thr=1.0d-4, 
/
&SYSTEM
  ibrav= 0, 
  nat= 6, 
  ntyp= 2,  
  ecutwfc = 50, 
  ecutrho = 500,
/
&ELECTRONS
  conv_thr = 1.0d-8
  mixing_beta = 0.7d0
/
&IONS
  ion_dynamics='bfgs' #this means you are doing 
/
&CELL
  cell_dynamics='bfgs'
  press=0.0
  press_conv_thr=0.5
/
ATOMIC_SPECIES
  C 12.0107 C.UPF
  H 1.00794 H.UPF
  #Errors may occur when you dont have ATOMIC_SPECIES UPF, you need to go to Quantum Espresso.com pseudo section, click on the element you want in the peroidic table, and find the most satisfied version of your UPF
  #Click on that, there will be a web link then use it!
CELL_PARAMETERS (angstrom)
  15.8753162577  0.0000000000  0.0000000000
   0.0000000000 15.8753162577  0.0000000000
   0.0000000000  0.0000000000  2.5702137021
ATOMIC_POSITIONS (crystal)
   H  0.4130446861  0.5134911523  0.2499998588
   H  0.4954148472  0.5878745676  0.2499998765
   C  0.4823608513  0.5195314039  0.2500002242
   C  0.5176391487  0.4804685961  0.7499997758
   H  0.5045851528  0.4121254324  0.7500001235
   H  0.5869553139  0.4865088477  0.7500001412
K_POINTS {automatic}
  1 1 9 0 0 0
  ```
  File name: /qe-6.7/polypedot/C2H4.vc_relax.in

###### Step2: scf calculations 
After vc_relax calculations, you can update your ATOMIC_POSITIONS 
```
&CONTROL
  calculation='scf', 
  restart_mode='from_scratch'
  pseudo_dir='../pseudo/', 
  prefix='C2H4'
  outdir='../tmp',  
  verbosity='high',
  tprnfor=.true., 
  tstress=.true.
  forc_conv_thr=1.0d-4, 
/
&SYSTEM
  ibrav= 0, 
  nat= 6, 
  ntyp= 2, 
  ecutwfc = 50, 
  ecutrho = 500,
/
&ELECTRONS
  conv_thr = 1.0d-8
  mixing_beta = 0.7d0
/
&IONS
/
&CELL
/
ATOMIC_SPECIES
  C 12.0107 C.UPF
  H 1.00794 H.UPF
CELL_PARAMETERS (angstrom)
   8.213159486  -0.312252721   0.000000000
  -0.312202472   8.272552668   0.000000000
   0.000000000   0.000000000   2.518404031
ATOMIC_POSITIONS (crystal)
H             0.3337483684        0.5196325621        0.2499998588
H             0.4972484968        0.6674413232        0.2499998765
C             0.4679216036        0.5355006009        0.2500002242
C             0.5320783964        0.4644993991        0.7499997758
H             0.5027515032        0.3325586768        0.7500001235
H             0.6662516316        0.4803674379        0.7500001412
K_POINTS {automatic}
  1 1 9 0 0 0
 ``` 
You will get ndnd number from the output file which is useful for next step
###### Step3: nscf calculations 

```
&CONTROL
  calculation='bands', 
  restart_mode='from_scratch'
  pseudo_dir='../pseudo/', 
  prefix='C2H4'
  outdir='../tmp',  
  verbosity='high',
  tprnfor=.true., 
  tstress=.true.
  forc_conv_thr=1.0d-4, 
/
&SYSTEM
  ibrav= 0, 
  nat= 6, 
  ntyp= 2, 
  ecutwfc = 50, 
  ecutrho = 500,
  nbnd=12
/
&ELECTRONS
  conv_thr = 1.0d-8
  mixing_beta = 0.7d0
/
&IONS
/
&CELL
/
ATOMIC_SPECIES
  C 12.0107 C.UPF
  H 1.00794 H.UPF
CELL_PARAMETERS (angstrom)
   8.213159486  -0.312252721   0.000000000
  -0.312202472   8.272552668   0.000000000
   0.000000000   0.000000000   2.518404031
ATOMIC_POSITIONS (crystal)
H             0.3337483684        0.5196325621        0.2499998588
H             0.4972484968        0.6674413232        0.2499998765
C             0.4679216036        0.5355006009        0.2500002242
C             0.5320783964        0.4644993991        0.7499997758
H             0.5027515032        0.3325586768        0.7500001235
H             0.6662516316        0.4803674379        0.7500001412
K_POINTS {automatic}
287
  ```
For K_Points, you need to installed Xcryden to choose your k-point path

###### Step4: band calculations 
