# Getting Started with Batch Job Scheduling: Slurm Edition

Batch job schedulers are used to manage and fairly distribute the shared resources of high-performance computing (HPC) systems. Learning how to interact with them and compose your work into batch jobs is essential to becoming an effective HPC user. 

In this tutorial, you will learn how to write your first batch job script and submit it to a batch job scheduler. You will also learn some best practices on how to structure your batch job scripts, leverage environment variables, and make better resource requests from the scheduler to get your work done faster. You will also be introduced to some advanced features like batch job arrays and dependencies that allow you to create more structured computational workflows.

## Disclaimer: :running: on :penguin:

HPC and advanced CI run on Linux. If you don't believe me, look no further than the latest statistics from the [TOP500](https://www.top500.org/statistics/list) --- a list of the most powerful supercomputers in the world. In this tutorial we will use --- *almost exclusively* --- standard command-line tools and applications that are available for [Unix-like](https://en.wikipedia.org/wiki/Unix-like) operating systems such as Linux and macOS. Recommendation for Windows users: Install the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl) on your personal computer at some point, if you haven't done so already. 

## Learning Goals

## Prerequistites

## Tutorial

- [Nohup No Mas: A quick review of managing Linux processes](NOHUP.md)
- [From Zero to Batch: Your first batch job and more](BATCH.md)
- [Getting Your Fair-Share: How to negotiate with the scheduler](FAIRSHARE.md)
- [Beyond the Basics: Batch job arrays and dependencies](BEYOND.md)

## Additional References

- [Batch Jobs @ SDSC Summer Insitute 2022](https://github.com/sdsc/sdsc-summer-institute-2022/blob/main/2.4_batch_computing/MThomas-SDSC-SI22-Batch-Jobs-July-27-Computing-Jul2022.pdf)
- [Slurm Quickstart User Guide](https://slurm.schedmd.com/quickstart.html)

#

[Marty Kandes](https://github.com/mkandes), Computational & Data Science Research Specialist, SDSC HPC User Services Group
