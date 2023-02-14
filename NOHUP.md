# Getting Started with Batch Job Scheduling: Slurm Edition

- [Nohup No Mas: A quick review of managing Linux processes](NOHUP.md)
- [From Zero to Batch: Your first batch job and more](BATCH.md)
- [Getting Your Fair-Share: How to negotiate with the scheduler](FAIRSHARE.md)
- [Beyond the Basics: Batch job arrays and dependencies](BEYOND.md)

## Nohup No Mas: A quick review of managing Linux processes

### What is a Process?

<img src='https://upload.wikimedia.org/wikipedia/commons/2/25/Concepts-_Program_vs._Process_vs._Thread.jpg' width='50%' height='50%'/>

A [process](https://en.wikipedia.org/wiki/Process_(computing)) is the instance of [computer program](https://en.wikipedia.org/wiki/Computer_program)
that is being executed by one or more [threads](https://en.wikipedia.org/wiki/Thread_(computing)).

### Estimating Pi

![Estimate the value of Pi via Monte Carlo](https://hpc.llnl.gov/sites/default/files/styles/no_sidebar_3_up/public/pi1.gif)

To explore managing procesess, we'll need an example program to run. For this tutorial, we'll be working with the [4pi](https://github.com/mkandes/4pi) project, which is a collection of simple computer programs that estimate the value of $\pi$. Each program in the collection differs only in the programming language it was written in, the set of features of the language it utilized, and/or the fundamental underlying mathematical algorithm it implemented to approximate the value of $\pi$. The aim of the project is to explore different aspects of each programming language and their feature sets from a scientific and high-performance computing perspective. The first set of programs included in the project estimate the value of $\pi$ via the Monte Carlo method. This solution is particularly useful for exploring different parallel programming models, languages, libraries, and APIs as it is an embarrassingly parallel (albeit inefficient) solution to the problem.


## Additional References

- https://en.wikipedia.org/wiki/Top_(software)
- https://en.wikipedia.org/wiki/Kill_(command)
- https://en.wikipedia.org/wiki/Nohup
- https://en.wikipedia.org/wiki/Nice_(Unix)

#

Next Section - [From Zero to Batch: Your first batch job and more](BATCH.md)
