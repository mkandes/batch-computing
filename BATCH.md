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
````

Then clone the [4pi](https://github.com/mkandes/4pi) repo to your `HOME` directory. 

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



Next Section - [Getting Your Fair-Share: How to negotiate with the scheduler](FAIRSHARE.md)
