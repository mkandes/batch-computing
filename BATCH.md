# Getting Started with Batch Job Scheduling: Slurm Edition

- [Nohup No Mas: Managing Linux processes](NOHUP.md)
- [From Zero to Batch: Your first batch job and more](BATCH.md)
- [Getting Your Fair-Share: How to negotiate with the scheduler](FAIRSHARE.md)
- [Beyond the Basics: Batch job arrays and dependencies](BEYOND.md)

## From Zero to Batch: Your first batch job and more

### Your Standard HPC Ecosystem

![standard-hpc-ecosystem](standard-hpc-ecosystem.jpg)

### HPC System Architecture

![expanse-system-architecture](expanse-system-architecture.png)

- [Expanse: Computing Without Boundaries](https://expanse.sdsc.edu)
- [Expanse: Building a Supercomputer](https://www.youtube.com/watch?v=uNZyg6X_t3s)

### HPC Storage Systems & External Networks

![expanse-storage-external-networks](expanse-storage-external-networks.png)

### Managing an HPC System: The Scheduler

[Slurm](https://en.wikipedia.org/wiki/Slurm_Workload_Manager) is an open source, fault-tolerant, and highly 
scalable HPC resource manager and job scheduling system.

![slurm-architecture](https://slurm.schedmd.com/arch.gif)

### Getting Started

Login to your HPC system.

*Command*

```
ssh username@login.expanse.sdsc.edu
```

*Output*

```
$ ssh username@login.expanse.sdsc.edu
Welcome to Bright release         9.0

                                                         Based on Rocky Linux 8
                                                                    ID: #000002

--------------------------------------------------------------------------------

                                 WELCOME TO
                  _______  __ ____  ___    _   _______ ______
                 / ____/ |/ // __ \/   |  / | / / ___// ____/
                / __/  |   // /_/ / /| | /  |/ /\__ \/ __/
               / /___ /   |/ ____/ ___ |/ /|  /___/ / /___
              /_____//_/|_/_/   /_/  |_/_/ |_//____/_____/

--------------------------------------------------------------------------------

Use the following commands to adjust your environment:

'module avail'            - show available modules
'module add <module>'     - adds a module to your environment for this session
'module initadd <module>' - configure module to be loaded at every login

-------------------------------------------------------------------------------
Last login: Wed Feb 15 14:35:40 2023 from 208.58.214.56
[username@login01 ~]$
```

And then take your first look around with `top`. 

![top-login-node](top-login-node.png)

You are not alone!

### The Rules of HPC Club

![top-bad-user](top-bad-user.png)

1. You do not run computationally-intensive work on the login nodes.
2. You DO NOT run computationally-intensive work on the login nodes.
3. All computationally-intensive work should be run as either a batch or interactive job on the compute nodes.
4. *To be continued* ...

### What is a Batch Job?

A batch job is a computer program or set of programs processed in batch mode, meaning they are typically executed without user interaction, 
and often at a time when the computer is otherwise idle.

In high-performance computing (HPC) environments, batch jobs are used to run large-scale simulations, perform complex data analysis, and 
run other types of demanding computational tasks. In an HPC environment, a batch job is typically submitted to a queue and managed by a job 
scheduler, which assigns the job to available resources and ensures that the job runs efficiently.

### Viewing Information About Jobs 

https://slurm.schedmd.com/squeue.html

*Command*

```
squeue
```

*Output*

```
[username@login01 ~]$ squeue
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
          20730227   compute NGBW-JOB   cipres PD       0:00      1 (ReqNodeNotAvail, Reserved for maintenance)
          20743722   compute   abaqus   wsang1 PD       0:00      1 (ReqNodeNotAvail, Reserved for maintenance)
          20753059   compute tio2_tem    rw624 PD       0:00     14 (QOSMaxCpuPerUserLimit)
          20753111   compute tio2_tem    rw624 PD       0:00     14 (QOSMaxCpuPerUserLimit)
          20753161   compute tio2_tem    rw624 PD       0:00     14 (QOSMaxCpuPerUserLimit)
          20753205   compute tio2_tem    rw624 PD       0:00     14 (QOSMaxCpuPerUserLimit)
          ...
          20754621   compute    scav2  apham78 PD       0:00      4 (Dependency)
          20754620   compute    scav2  apham78 PD       0:00      4 (Dependency)
          20754619   compute    scav2  apham78 PD       0:00      4 (Dependency)
          ...
          20753239   compute tio2_tem    rw624 PD       0:00     14 (QOSMaxCpuPerUserLimit)
          20753215   compute tio2_tem    rw624 PD       0:00     14 (QOSMaxCpuPerUserLimit)
          20677210   compute D8.00_q0    healy PD       0:00     10 (Dependency)
          20677209   compute D8.00_q0    healy PD       0:00     10 (Dependency)
          20677230   compute D24.55_q    healy PD       0:00      6 (Dependency)
          ...
          20677216   compute D24.55_q    healy PD       0:00      6 (Dependency)
          18495201   compute NGBW-JOB  nsguser PD       0:00      1 (launch failed requeued held)
          20722553   compute Nf2L28Ru nkarthik  R 1-23:53:09      1 exp-3-22
          20723048   compute   nozzle  ajavadi  R 1-23:05:26      4 exp-8-[37-40]
          20723398   compute Nf2L28Ru nkarthik  R 1-22:33:52      1 exp-2-15
          20724107   compute Nf2L28Ru nkarthik  R 1-22:27:51      1 exp-2-34
          ...
          20726357   compute Nf6L28Ru nkarthik  R 1-21:51:47      1 exp-2-01
          20726375   compute Nf6L28Ru nkarthik  R 1-21:50:47      1 exp-14-27
          20726393   compute Nf6L28Ru nkarthik  R 1-21:47:47      1 exp-2-02
          20728441   compute test.scr ux451202  R 1-18:56:51     16 exp-14-[33-48]
          ...
          20756896    shared NGBW-JOB   cipres  R       1:16      1 exp-1-35
          20756920    shared NGBW-JOB   cipres  R       1:16      1 exp-1-31
          20756874    shared S724141_ zhanglab  R       1:16      1 exp-1-15
          20756881    shared S724141_ zhanglab  R       1:16      1 exp-1-29
          20756890    shared S724141_ zhanglab  R       1:16      1 exp-1-29
          20756900    shared S724141_ zhanglab  R       1:16      1 exp-1-29
          20756911    shared S724141_ zhanglab  R       1:16      1 exp-1-29
          20756923    shared S724141_ zhanglab  R       1:16      1 exp-1-29
          20756934    shared S724141_ zhanglab  R       1:16      1 exp-1-29
[username@login01 ~]$
```

### Viewing Information About Nodes

https://slurm.schedmd.com/sinfo.html

*Command*

```
sinfo
```

*Output*

```
[username@login01 ~]$ sinfo
PARTITION      AVAIL  TIMELIMIT  NODES  STATE NODELIST
compute           up 2-00:00:00      1  drng@ exp-2-40
compute           up 2-00:00:00      4 drain$ exp-8-[13-16]
compute           up 2-00:00:00      2 drain* exp-13-[55-56]
compute           up 2-00:00:00      8   resv exp-16-[53-56],exp-17-[53-56]
compute           up 2-00:00:00    124    mix exp-1-[01-24,26-27,29-33,35-38,40-45,48,50,53-56],exp-2-[04-05,07,09,12,14,16,18,21,25-26,35,38,41,45-46,53-54,56],exp-3-[01-02,09,11,19,22,25,28,33,39,45-46,53-55],exp-4-[14,22,25-26,56],exp-5-[31,42,45-46],exp-6-[15,18,22,39-40,49,52],exp-7-[01,18,20,24],exp-8-[01,04,06,08-09,12,35-36,41],exp-9-[16,20,51],exp-10-[19-22,56],exp-13-[50-54],exp-14-29
compute           up 2-00:00:00    375  alloc exp-1-[25,28,34,39,46-47,49],exp-2-[01-03,06,08,10-11,13,15,17,19-20,22-24,27-34,36-37,39,42-44,47-52,55],exp-3-[03-08,10,12-18,23-24,26-27,29-32,34-38,40-44,47-52,56],exp-4-[01-13,15-21,23-24,27-55],exp-5-[01-30,32-34,43-44,47],exp-6-[01-12,16-17,19-21,23-38,41-48,50-51,53-56],exp-7-[02-17,19,21-23,25],exp-8-[02-03,05,07,10-11,17-34,37-40,42-56],exp-9-[01-15,17-19,21-26,42-50,52-54],exp-10-[01-18,23-27,34-35,47,55],exp-12-[01-03,51-53],exp-13-[28,45-49],exp-14-[27-28,33-50]
compute           up 2-00:00:00    220   idle exp-1-[51-52],exp-3-[20-21],exp-5-[35-41,48-56],exp-6-[13-14],exp-7-[26-56],exp-9-[27-41],exp-10-[28-33,36-46,48-54],exp-12-[04-50,54-56],exp-13-[01-27,29-44],exp-14-[01-26,30-32,51-56]
debug             up      30:00      1    mix exp-9-56
debug             up      30:00      1  alloc exp-9-55
gpu               up 2-00:00:00      1  drng@ exp-2-57
gpu               up 2-00:00:00      6   resv exp-16-[57-59],exp-17-[57-59]
gpu               up 2-00:00:00     22    mix exp-1-[57,59-60],exp-2-60,exp-3-59,exp-4-[57,59-60],exp-5-[58,60],exp-6-[57,59],exp-7-58,exp-8-[57-58],exp-9-[57-58],exp-12-[58-59],exp-13-[57-58],exp-14-58
gpu               up 2-00:00:00     27  alloc exp-1-58,exp-2-[58-59],exp-3-[57-58,60],exp-4-58,exp-5-[57,59],exp-6-[58,60],exp-7-57,exp-8-[59-60],exp-9-[59-60],exp-10-[57-60],exp-12-[57,60],exp-13-[59-60],exp-14-[57,59-60]
gpu-debug         up      30:00      1    mix exp-7-59
gpu-debug         up      30:00      1   idle exp-7-60
gpu-preempt       up 7-00:00:00      1  drng@ exp-2-57
gpu-preempt       up 7-00:00:00     22    mix exp-1-[57,59-60],exp-2-60,exp-3-59,exp-4-[57,59-60],exp-5-[58,60],exp-6-[57,59],exp-7-58,exp-8-[57-58],exp-9-[57-58],exp-12-[58-59],exp-13-[57-58],exp-14-58
gpu-preempt       up 7-00:00:00     27  alloc exp-1-58,exp-2-[58-59],exp-3-[57-58,60],exp-4-58,exp-5-[57,59],exp-6-[58,60],exp-7-57,exp-8-[59-60],exp-9-[59-60],exp-10-[57-60],exp-12-[57,60],exp-13-[59-60],exp-14-[57,59-60]
gpu-shared        up 2-00:00:00      1  drng@ exp-2-57
gpu-shared        up 2-00:00:00      6   resv exp-16-[57-59],exp-17-[57-59]
gpu-shared        up 2-00:00:00     22    mix exp-1-[57,59-60],exp-2-60,exp-3-59,exp-4-[57,59-60],exp-5-[58,60],exp-6-[57,59],exp-7-58,exp-8-[57-58],exp-9-[57-58],exp-12-[58-59],exp-13-[57-58],exp-14-58
gpu-shared        up 2-00:00:00     27  alloc exp-1-58,exp-2-[58-59],exp-3-[57-58,60],exp-4-58,exp-5-[57,59],exp-6-[58,60],exp-7-57,exp-8-[59-60],exp-9-[59-60],exp-10-[57-60],exp-12-[57,60],exp-13-[59-60],exp-14-[57,59-60]
ind-compute       up 2-00:00:00      1  drng@ exp-15-02
ind-compute       up 2-00:00:00      1   resv exp-15-56
ind-compute       up 2-00:00:00      8    mix exp-15-[03-07,09-11]
ind-compute       up 2-00:00:00      1  alloc exp-15-01
ind-compute       up 2-00:00:00     45   idle exp-15-[08,12-55]
ind-gpu           up 2-00:00:00      1  inval exp-15-60
ind-gpu           up 2-00:00:00      1   resv exp-15-57
...
ind-shared        up 2-00:00:00      1  alloc exp-15-01
ind-shared        up 2-00:00:00     45   idle exp-15-[08,12-55]
large-shared      up 2-00:00:00      4   idle exp-11-[01-04]
preempt           up 7-00:00:00      1  drng@ exp-2-40
preempt           up 7-00:00:00      4 drain$ exp-8-[13-16]
preempt           up 7-00:00:00      2 drain* exp-13-[55-56]
...
preempt           up 7-00:00:00    220   idle exp-1-[51-52],exp-3-[20-21],exp-5-[35-41,48-56],exp-6-[13-14],exp-7-[26-56],exp-9-[27-41],exp-10-[28-33,36-46,48-54],exp-12-[04-50,54-56],exp-13-[01-27,29-44],exp-14-[01-26,30-32,51-56]
shared*           up 2-00:00:00      1  drng@ exp-2-40
shared*           up 2-00:00:00      4 drain$ exp-8-[13-16]
shared*           up 2-00:00:00      2 drain* exp-13-[55-56]
shared*           up 2-00:00:00      8   resv exp-16-[53-56],exp-17-[53-56]
shared*           up 2-00:00:00    124    mix exp-1-[01-24,26-27,29-33,35-38,40-45,48,50,53-56],exp-2-[04-05,07,09,12,14,16,18,21,25-26,35,38,41,45-46,53-54,56],exp-3-[01-02,09,11,19,22,25,28,33,39,45-46,53-55],exp-4-[14,22,25-26,56],exp-5-[31,42,45-46],exp-6-[15,18,22,39-40,49,52],exp-7-[01,18,20,24],exp-8-[01,04,06,08-09,12,35-36,41],exp-9-[16,20,51],exp-10-[19-22,56],exp-13-[50-54],exp-14-29
shared*           up 2-00:00:00    375  alloc exp-1-[25,28,34,39,46-47,49],exp-2-[01-03,06,08,10-11,13,15,17,19-20,22-24,27-34,36-37,39,42-44,47-52,55],exp-3-[03-08,10,12-18,23-24,26-27,29-32,34-38,40-44,47-52,56],exp-4-[01-13,15-21,23-24,27-55],exp-5-[01-30,32-34,43-44,47],exp-6-[01-12,16-17,19-21,23-38,41-48,50-51,53-56],exp-7-[02-17,19,21-23,25],exp-8-[02-03,05,07,10-11,17-34,37-40,42-56],exp-9-[01-15,17-19,21-26,42-50,52-54],exp-10-[01-18,23-27,34-35,47,55],exp-12-[01-03,51-53],exp-13-[28,45-49],exp-14-[27-28,33-50]
shared*           up 2-00:00:00    220   idle exp-1-[51-52],exp-3-[20-21],exp-5-[35-41,48-56],exp-6-[13-14],exp-7-[26-56],exp-9-[27-41],exp-10-[28-33,36-46,48-54],exp-12-[04-50,54-56],exp-13-[01-27,29-44],exp-14-[01-26,30-32,51-56]
[username@login01 ~]$
```

### Clone the Repository

Next, let's clone the [4pi](https://github.com/mkandes/4pi) repo to your `HOME` directory on the HPC system.

*Command*

```
git clone https://github.com/mkandes/4pi.git
```

*Output*

```
[username@login01 ~]$ git clone https://github.com/mkandes/4pi.git
Cloning into '4pi'...
remote: Enumerating objects: 19, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 19 (delta 4), reused 19 (delta 4), pack-reused 0
Unpacking objects: 100% (19/19), 5.72 KiB | 7.00 KiB/s, done.
[username@login01 ~]$
```

### Your First Batch Job

To create your first batch job script, you'll need to open a new file in your text editor of choice.

```
[username@login01 ~]$ vi run-4pi.sh
```

```
#!/usr/bin/env bash

#SBATCH --job-name=my-first-4pi-test
#SBATCH --account=abc123
#SBATCH --partition=debug
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=1G
#SBATCH --time=00:05:00
#SBATCH --output=%x.o%j.%N

python3 pi.py 100000000
```

### More Examples

```
[username@login01 ~]$ ls /cm/shared/examples/sdsc/
abaqus     dftbplus  lammps             nwchem    qe          vasp
abinit     excerpt   localscratch       openacc   raxml       vasp-ase
alphafold  gamess    matlab             openmp    si          visit
amber      gaussian  mpi                orca      siesta      wannier90
bintest    gromacs   mpi-openmp-hybrid  paraview  spark       xpmem
ciml       hadoop    namd               pyscf     tensorflow
classes    hpcg      neuron             pytorch   test
cp2k       hpl       nsight             qchem     trinity
[mkandes@login01 ~]$
```

Next Section - [Getting Your Fair-Share: How to negotiate with the scheduler](FAIRSHARE.md)
