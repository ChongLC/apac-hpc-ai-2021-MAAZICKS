# Group MAAZICKS - 2021 APAC HPC-AI Competition
This repository is particularly for the documentation of 2021 APAC HPC-AI Competition, specifically for group MAAZICKS.

---
## Task 1 
High-Performance Computing – **GROMACS** (GROningen MAchine for Chemical Simulations)

### Goal: 
- [ ] Explain how Gromacs was configured, compiled and deployed Makefile
- [ ] Showcase the ways on running the MD 
- [ ] Optimise the performance (ns/day) and wall time (s)

### Input file
* portable binary run input file that contains the starting structure of your simulation, the molecular topology and all the simulation parameters.  

### Required package
* GROMACS

  Note: To check the package in server (linux-based) <br>
  ```
  module avail
  ```

### Methodology 
**Step 1: Load GROMACS** <br>
`module load {gromacs-package-PATH}`

**Step 2: Run MD** <br>
`mpirun -np {number of processors available} mdrun_mpi -s {input file} -v -noconfout -resethway -nsteps {number of steps} -ntomp {number of omp threads} -gcom {numbers before communcation phases} -nstlist {numbers before list converging}`

### Output
Please refer to the [HPC-GROMACS](https://github.com/ChongLC/apac-hpc-ai-2021-MAAZICKS/tree/main/HPC-GROMACS) folder.

---
## Team members
[Li Chuin Chong](https://github.com/ChongLC) (Team Lead), Bezmialem Vakıf Universty <br>
Jeremias Ivan, Indonesia International Institute for Life Sciences <br>
[Muhammad Zainul Arifin Nasution](https://github.com/ZainulArifin1), Indonesia International Institute for Life Sciences <br>
[Stefanus Bernard](https://github.com/Gatchmon), Indonesia International Institute for Life Sciences <br>
Aakib Bin Nesar, North South University Bangladesh <br>
[Eo Yeo Keat](https://github.com/yeokeat), University Putra Malaysia

## Advisors
[Kenneth Ban Hon Kim](https://github.com/kennethban) (Advisor), National University of Singapore <br>
[Ayesha Fatima](https://github.com/ayeshafatma) (Advisor), Bezmialem Vakıf Universty <br>
