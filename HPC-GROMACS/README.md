## Task 1 
High-Performance Computing – **GROMACS** (GROningen MAchine for Chemical Simulations) <br>

### Goal: 
- [ ] Explain how Gromacs was configured, compiled and deployed Makefile <br> Note: As the NSCC server has GROMACS, we directly used that via `module load` command. 
- [x] Showcase the ways on running the MD 
- [x] Optimise the performance (ns/day) and wall time (s)

### Input file
* lignocellulose-tf
* stmv

### Methodology 
**Step 1: Load GROMACS** <br>
```
module load gromacs/2018.2/gcc493/impi
```

**Step 2: Run MD** <br> 
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
 Domain decomp.        24    1       1667     116.687       7264.381   1.4
 DD comm. load         24    1        169       0.071          4.448   0.0
 Neighbor search       24    1       1667     524.196      32633.850   6.1
 Comm. coord.          24    1      48334     101.271       6304.623   1.2
 Force                 24    1      50001    7303.220     454662.588  84.6
 Wait + Comm. F        24    1      50001     171.699      10689.123   2.0
 NB X/F buffer ops.    24    1     146669      79.698       4961.582   0.9
 Update                24    1      50001      94.828       5903.505   1.1
 Constraints           24    1      50001     228.387      14218.273   2.6
 Comm. energies        24    1       2001      11.385        708.786   0.1
 Rest                                           5.821        362.397   0.1
-----------------------------------------------------------------------------
 Total                                       8637.263     537713.557 100.0
-----------------------------------------------------------------------------

               Core t (s)   Wall t (s)        (%)
       Time:   207294.314     8637.263     2400.0
                         2h23:57
                 (ns/day)    (hour/ns)
Performance:        1.000       23.992
Finished mdrun on rank 0 Thu Oct 14 04:39:48 2021
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
