# QE-Tutorial

###### Step1: Install qe-6.7; Install gfortran and C++ at HPC; Install openmpi:
```
(base) Yixins-MacBook-Pro-2:~ yixinli$ cd Downloads

(base) Yixins-MacBook-Pro-2:Downloads yixinli$ gunzip gcc-10.2-bin.tar.gz

(base) Yixins-MacBook-Pro-2:Downloads yixinli$ sudo tar -xvf gcc-10.2-bin.tar -C /

then, download openmpi-4.0.5 from web

(base) Yixins-MacBook-Pro-2:~ yixinli$ cd Downloads

(base) Yixins-MacBook-Pro-2:Downloads yixinli$ cd openmpi-4.0.5

(base) Yixins-MacBook-Pro-2:openmpi-4.0.5 yixinli$ make all install
```
```
(base) Yixins-MacBook-Pro-2:~ yixinli$ cd QE

(base) Yixins-MacBook-Pro-2:QE yixinli$ cd qe-6.7

(base) Yixins-MacBook-Pro-2:qe-6.7 yixinli$ ./configure

(base) Yixins-MacBook-Pro-2:qe-6.2 yixinli$ make all
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

$ curl -o C.UPF https://www.quantum-espresso.org/upf_files/C.pz-rrkjus.UPF
```
or 
```
mpirun -np 2 ~/QE/qe-6.7/bin/pw.x inp si.vc_relax > si.vc_relax.out
```
 > Reference: https://www.youtube.com/watch?v=uWhZOwfmV2s&ab_channel=PhysWhiz

Basically this means we've completed our installations

----------------------------------------------------------------

First, Let's start calculations on band energies:

###### Transfer VASP file into Scf file 

(transfer direct coordinates into Cartesian coordinates)

 > https://chemistry.stackexchange.com/questions/136836/converting-fractional-coordinates-into-cartesian-coordinates-for-crystallography

###### Step1: vc-relax (结构优化）
cif into QE Coordinates  

###### Step2: scf calculations 
###### Step3: nscf calculations 
###### Step4: band calculations 
