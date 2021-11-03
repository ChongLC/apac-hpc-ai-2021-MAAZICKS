## Task 1 
High-Performance Computing – **GROMACS** (GROningen MAchine for Chemical Simulations) <br>

### Goal: 
- [ ] Explain how Gromacs was configured, compiled and deployed Makefile <br> Note: As the NSCC server has GROMACS, we directly used that via `module load` command. 
- [x] Showcase the ways on running the MD 
- [x] Optimise the performance (ns/day) and wall time (s)

### Input file
* [lignocellulose-tf](https://github.com/ChongLC/apac-hpc-ai-2021-MAAZICKS/blob/main/HPC-GROMACS/lignocellulose-rf/lignocellulose-rf.zip)
* [stmv](https://github.com/ChongLC/apac-hpc-ai-2021-MAAZICKS/blob/main/HPC-GROMACS/stmv/stmv.zip)

### Methodology 
**Step 1: Load GROMACS** <br>
GROMACS v2018 was applied herein. 
```
module load gromacs/2018.2/gcc493/impi
```

**Step 2: Run MD** <br> 
Normal MD code:
```
# for lignocellulose-tf
mpirun -np 24 mdrun_mpi -s lignocellulose-rf.tpr -v -noconfout -resethway -nsteps 100000 -ntomp 1
```
To improve the performance, we have added two parameters: gcom and nstlist. 

Optimised MD code: 
```
# for lignocellulose-tf
mpirun -np 24 mdrun_mpi -s lignocellulose-rf.tpr -v -noconfout -resethway -nsteps 100000 -ntomp 1 -gcom 30 -nstlist 30

# for stmv
mpirun -np 24 mdrun_mpi -s stmv.tpr -v -noconfout -resethway -nsteps 100000 -ntomp 1 -gcom 20 -nstlist 20
```

### Output
For lignocellulose-tf, 
```
     R E A L   C Y C L E   A N D   T I M E   A C C O U N T I N G

On 24 MPI ranks

 Computing:          Num   Num      Call    Wall time         Giga-Cycles
                     Ranks Threads  Count      (s)         total sum    %
-----------------------------------------------------------------------------
 Domain decomp.        24    1       1429      98.509       6132.741   1.1
 DD comm. load         24    1       1267       0.448         27.871   0.0
 DD comm. bounds       24    1       1258       2.903        180.728   0.0
 Neighbor search       24    1       1429     473.238      29461.591   5.5
 Comm. coord.          24    1      48572      93.771       5837.779   1.1
 Force                 24    1      50001    7330.733     456377.656  85.6
 Wait + Comm. F        24    1      50001     144.688       9007.583   1.7
 NB X/F buffer ops.    24    1     147145      85.305       5310.667   1.0
 Update                24    1      50001     107.475       6690.889   1.3
 Constraints           24    1      50001     217.162      13519.503   2.5
 Comm. energies        24    1       1859       8.911        554.733   0.1
 Rest                                           5.407        336.642   0.1
-----------------------------------------------------------------------------
 Total                                       8568.549     533438.384 100.0
-----------------------------------------------------------------------------

               Core t (s)   Wall t (s)        (%)
       Time:   205645.180     8568.549     2400.0
                         2h22:48
                 (ns/day)    (hour/ns)
Performance:        1.008       23.801
Finished mdrun on rank 0 Tue Oct 19 00:37:41 2021
```

For stmv, 
```
     R E A L   C Y C L E   A N D   T I M E   A C C O U N T I N G

On 20 MPI ranks doing PP, and
on 4 MPI ranks doing PME

 Computing:          Num   Num      Call    Wall time         Giga-Cycles
                     Ranks Threads  Count      (s)         total sum    %
-----------------------------------------------------------------------------
 Domain decomp.        20    1       2501     139.058       7214.235   1.8
 DD comm. load         20    1       2501       0.223         11.574   0.0
 DD comm. bounds       20    1       2501       0.815         42.297   0.0
 Send X to PME         20    1      50001      27.771       1440.721   0.4
 Neighbor search       20    1       2501     223.967      11619.277   2.9
 Comm. coord.          20    1      47500      35.600       1846.921   0.5
 Force                 20    1      50001    5730.158     297276.687  73.6
 Wait + Comm. F        20    1      50001      80.230       4162.287   1.0
 PME mesh *             4    1      50001    2346.884      24350.947   6.0
 PME wait for PP *                           4142.641      42983.476  10.6
 Wait + Recv. PME F    20    1      50001      17.333        899.232   0.2
 NB X/F buffer ops.    20    1     145001      27.183       1410.247   0.3
 Update                20    1      50001      25.798       1338.378   0.3
 Constraints           20    1      50001     177.904       9229.516   2.3
 Comm. energies        20    1       2501       0.981         50.881   0.0
 Rest                                           2.505        129.972   0.0
-----------------------------------------------------------------------------
 Total                                       6489.527     404006.669 100.0
-----------------------------------------------------------------------------
(*) Note that with separate PME ranks, the walltime column actually sums to
    twice the total reported, but the cycle count total and % are correct.
-----------------------------------------------------------------------------
 Breakdown of PME mesh computation
-----------------------------------------------------------------------------
 PME redist. X/F        4    1     100002     173.706       1802.347   0.4
 PME spread             4    1      50001     828.669       8598.158   2.1
 PME gather             4    1      50001     577.557       5992.651   1.5
 PME 3D-FFT             4    1     100002     603.841       6265.366   1.6
 PME 3D-FFT Comm.       4    1     100002     108.217       1122.847   0.3
 PME solve Elec         4    1      50001      54.557        566.072   0.1
-----------------------------------------------------------------------------

               Core t (s)   Wall t (s)        (%)
       Time:   155748.652     6489.527     2400.0
                         1h48:09
                 (ns/day)    (hour/ns)
Performance:        1.331       18.026
Finished mdrun on rank 0 Tue Oct 12 12:34:19 2021
```
