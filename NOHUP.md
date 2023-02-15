# Getting Started with Batch Job Scheduling: Slurm Edition

- [Nohup No Mas: A quick review of managing Linux processes](NOHUP.md)
- [From Zero to Batch: Your first batch job and more](BATCH.md)
- [Getting Your Fair-Share: How to negotiate with the scheduler](FAIRSHARE.md)
- [Beyond the Basics: Batch job arrays and dependencies](BEYOND.md)

## Nohup No Mas: A quick review of managing Linux processes

### What is a Process?

<img src='https://upload.wikimedia.org/wikipedia/commons/2/25/Concepts-_Program_vs._Process_vs._Thread.jpg' width='75%' height='75%'/>

A [process](https://en.wikipedia.org/wiki/Process_(computing)) is the instance of [computer program](https://en.wikipedia.org/wiki/Computer_program)
that is being executed by one or more [threads](https://en.wikipedia.org/wiki/Thread_(computing)).

### Example Program: Estimating $\pi$ via Monte Carlo

![Estimate the value of Pi via Monte Carlo](https://hpc.llnl.gov/sites/default/files/styles/no_sidebar_3_up/public/pi1.gif)

To explore managing procesess, we'll need an example program to run. Here, we'll start by working with the [4pi](https://github.com/mkandes/4pi) project,
a collection of simple computer programs that estimate the value of $\pi$. Each program in the collection differs only in the programming language 
it was written in, the set of features of the language it utilized, and/or the fundamental underlying mathematical algorithm it implemented to approximate
the value of $\pi$. The aim of the project is to explore different aspects of each programming language and their feature sets from a scientific and 
high-performance computing perspective. The first set of programs included in the project estimate the value of $\pi$ via the Monte Carlo method. This 
solution is particularly useful for exploring different parallel programming models, languages, libraries, and APIs as it is an embarrassingly parallel
(albeit inefficient) solution to the problem.

### Clone the Repository

Start by cloning the repository to your local system.

*Command*
  ```
  git clone https://github.com/mkandes/4pi.git
  ```

*Output*
```
$ git clone https://github.com/mkandes/4pi.git
Cloning into '4pi'...
remote: Enumerating objects: 19, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 19 (delta 4), reused 19 (delta 4), pack-reused 0
Unpacking objects: 100% (19/19), 5.72 KiB | 977.00 KiB/s, done.
```

### Explore the Repository

Take a look at the code available in the repository. 

```
$ cd 4pi/
$ ls
bash  c  fortran  LICENSE.md  python  README.md
$ ls python/
pi.py
$ ls fortran/
Makefile  pi.f90  pi_omp.f90
$ cat fortran/Makefile 
COMPILER := gfortran
COMPILER_OPTIONS := -ffree-form -ffree-line-length-none -fimplicit-none \
                    -O3 -mtune=native -fdefault-integer-8 -fdefault-real-8

all: pi.x pi_omp.x

pi.x: pi.o
	$(COMPILER) $(COMPILER_OPTIONS) -o pi.x pi.o

pi.o: pi.f90
	$(COMPILER) $(COMPILER_OPTIONS) -c pi.f90

pi_omp.x: pi_omp.o
	$(COMPILER) $(COMPILER_OPTIONS) -fopenmp -o pi_omp.x pi_omp.o

pi_omp.o: pi_omp.f90
	$(COMPILER) $(COMPILER_OPTIONS) -fopenmp -c pi_omp.f90

.PHONY: clean
clean:
	rm *.x *.o
$ ls c/
pi.c
$ cd bash/
$ ls bash/
pi.sh
```

### Working with bash/pi.sh

We'll start with the `pi.sh` shell script, which has three different command-line options that must be specified at runtime.

*Command*

```
head -n 15 pi.sh
```

*Output*

```
$ head -n 15 pi.sh 
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
```

### Estimating $\pi$ for the first time

Run `pi.sh` with these initial input parameters, then increase the number of samples (`-s`) used to estimate $\pi$.

*Command* 

```
./pi.sh -b 8 -r 5 -s 1000
```

*Output*

```
$ ./pi.sh -b 8 -r 5 -s 1000
3.15200
```

### Measuring Runtime

Measure the runtime of the program with the [`time`](https://en.wikipedia.org/wiki/Time_%28Unix%29) command.

*Command* 

```
time -p ./pi.sh -b 8 -r 5 -s 1000
```

*Output*

```
$ time -p ./pi.sh -b 8 -r 5 -s 1000
3.18400
real 6.13
user 5.37
sys 1.13
```

### Background vs. Foreground Processes

A **foreground process** is a process that runs a shell command or program immediately upon instantiation by a user and may have its 
[input/output (I/O)](https://en.wikipedia.org/wiki/Input/output) directly connected to either a 
[command-line interface (CLI)](https://en.wikipedia.org/wiki/Command-line_interface) or a 
[graphical user interface (GUI)](https://en.wikipedia.org/wiki/Graphical_user_interface) to communicate with the user. All shell commands and 
user programs run as foregound processes by default. Foreground processes may also sometimes be referred to as *interactive processes*. 
Examples include your operating system's terminal application, your web broswer, and your video conferencing software.

In contrast, a [**background process**](https://en.wikipedia.org/wiki/Background_process) is a process that runs a program independently of 
any user interaction. As such, once instantiated, you don't have to wait for the process to complete to execute another one. Background 
processes may also sometime be referred to as *non-interactive processes*. Any software application that runs a 
[daemon](https://en.wikipedia.org/wiki/Daemon_(computing)) is utilizing background processes. Examples include the SSH server-side 
application that provides you remote access to your lab's workstation computer and the web server hosting this tutorial.

### View Running Processes

The [`top`](https://en.wikipedia.org/wiki/Top_(software)) command displays information about running processes such as 
CPU and memory utilization. The most *active*, resource-intensive processes are shown at the top of table.

![top](top.png)

In addition to providing summary resource usage statistics for the computer system as a whole, `top` displays the 
following information for each process by default:

- `PID`: Unique Process ID given to each process.
- `USER`: Username of the process owner.
- `PR`: Priority given to a process while scheduling.
- `NI`: ‘nice’ value of a process.
- `VIRT`: Amount of virtual memory used by a process.
- `RES`: Amount of physical memory used by a process.
- `SHR`: Amount of memory shared with other processes.
- `S`: state of the process 
- `%CPU`: Percentage of CPU used by the process.
- `%MEM` :Percentage of RAM used by the process.
- `TIME+`: Total CPU time consumed by the process.
- `COMMAND`: Command used to activate the process.

### Interrupt a Foreground Process from the Keyboard

Re-run `pi.sh` with a large number of samples to increase its runtime (and accuracy) and then open another terminal to view the running process
with the `top` command.

*Command: Terminal 1* 

```
time -p ./pi.sh -b 8 -r 5 -s 100000
```

*Command: Terminal 2* 

```
top -u $USER
```

*Output: Terminal 2*

```
top - 12:05:06 up 1 day,  5:26,  1 user,  load average: 1.42, 1.62, 1.29
Tasks: 304 total,   1 running, 302 sleeping,   0 stopped,   1 zombie
%Cpu(s):  7.3 us, 11.2 sy,  0.0 ni, 81.4 id,  0.1 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :  31820.4 total,  18022.0 free,   5115.7 used,   8682.7 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.  25209.0 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND  
1091257 mkandes   20   0    9500   3304   3036 S  12.0   0.0   0:08.19 bash
...
```

Return to the terminal running `pi.sh` and interrupt the process by holding down the `ctrl` key with the letter `C` on your keyboard, 
which sends a `SIGINT` [signal](https://en.wikipedia.org/wiki/Signal_(IPC)) to the process and terminiates it immediately.

*Output: Terminal 1*

```
$ time -p ./pi.sh -b 8 -r 5 -s 100000
^C
real 169.53
user 148.93
sys 31.79
```

### Background a Process

You can run any command from your terminal as a background process by appending it with a space and then the ampersand (`&`) symbol. For example,
re-run `pi.sh` with a large number of samples again.

*Command* 

```
time -p ./pi.sh -b 8 -r 5 -s 100000 &
```

*Output*

```
$ time -p ./pi.sh -b 8 -r 5 -s 100000 &
[1] 2737701
$
```

After printing the`PID` of the background process to standard output, the shell prompt is returned to your control, where you can run 
another command such as viewing the background process with `top`. 

*Command*

```
top -p "$(($!+1))"
```

*Output*

```
Tasks:   1 total,   0 running,   1 sleeping,   0 stopped,   0 zombie
%Cpu(s):  9.4 us,  9.4 sy,  0.0 ni, 81.2 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :  31820.4 total,  17271.1 free,   5583.0 used,   8966.3 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.  24732.1 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND  
2737701 mkandes   20   0    9500   3404   3136 S  13.3   0.0   0:16.69 bash
```

Here, we've used the special environment variable `$!`, which returns the `PID` of the last executed command. Why do we add `+1` to this 
integer value?

### How to Kill a Background Process

While `top` continues to run on your `bash` command from `pi.sh`, open a new terminal and 
[`kill`](https://en.wikipedia.org/wiki/Kill_(command)) the process. 

*Command*

```
kill  2737701
```

When executed, you should see the process `Terminated` and disappear from `top`'s process table. Exit `top` and restart `pi.sh`. 

*Command*

```
$ time -p ./pi.sh -b 8 -r 5 -s 100000 &
[2] 3990461
```

Then open another terminal again and use `top` to display the process.

*Command*

```
top -p 3990462
```

What happens if you close the terminal where you originally restarted `pi.sh`?


## Additional References

- https://en.wikipedia.org/wiki/Nohup
- https://en.wikipedia.org/wiki/Nice_(Unix)

#

Next Section - [From Zero to Batch: Your first batch job and more](BATCH.md)
