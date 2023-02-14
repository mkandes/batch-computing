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

### Estimating $\pi$

![Estimate the value of Pi via Monte Carlo](https://hpc.llnl.gov/sites/default/files/styles/no_sidebar_3_up/public/pi1.gif)

To explore managing procesess, we'll need an example program to run. Here, we'll start by working with the [4pi](https://github.com/mkandes/4pi) project,
a collection of simple computer programs that estimate the value of $\pi$. Each program in the collection differs only in the programming language 
it was written in, the set of features of the language it utilized, and/or the fundamental underlying mathematical algorithm it implemented to approximate
the value of $\pi$. The aim of the project is to explore different aspects of each programming language and their feature sets from a scientific and 
high-performance computing perspective. The first set of programs included in the project estimate the value of $\pi$ via the Monte Carlo method. This 
solution is particularly useful for exploring different parallel programming models, languages, libraries, and APIs as it is an embarrassingly parallel
(albeit inefficient) solution to the problem.

### Clone the Repo to Your Local System

Start by cloning the repository to your local system.

*Command*
  ```
  git clone https://github.com/mkandes/4pi.git
  ```

*Output*
```
mkandes@hardtack:~$ git clone https://github.com/mkandes/4pi.git
Cloning into '4pi'...
remote: Enumerating objects: 19, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 19 (delta 4), reused 19 (delta 4), pack-reused 0
Unpacking objects: 100% (19/19), 5.72 KiB | 977.00 KiB/s, done.
mkandes@hardtack:~$
```

### bash/pi.sh

We'll start with the `bash/pi.sh` shell script, which has three different command-line options that are required to be specified at runtime.

*Command*

```
head -n 15 4pi/bash/pi.sh
```

*Output*

```
mkandes@hardtack:~$ head -n 15 4pi/bash/pi.sh 
#!/usr/bin/env bash
#
# Estimate the value of Pi via Monte Carlo

# Read in and parse input variables from command-line arguments
if (( "${#}" > 0 )); then
  while (( "${#}" > 0 )); do
    case "${1}" in
      -b | --bytes ) bytes="${2}" ;;
      -r | --round ) round="${2}" ;;
      -s | --samples ) samples="${2}" ;;
    esac
    shift 2
  done
fi
mkandes@hardtack:~$
```

## Additional References

- https://en.wikipedia.org/wiki/Top_(software)
- https://en.wikipedia.org/wiki/Kill_(command)
- https://en.wikipedia.org/wiki/Nohup
- https://en.wikipedia.org/wiki/Nice_(Unix)

#

Next Section - [From Zero to Batch: Your first batch job and more](BATCH.md)
