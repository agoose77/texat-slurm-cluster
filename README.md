# README
## Setup
The `modulefiles` directory contains the Environment Modulefile for `texat`. It must be edited to reflect the full path to the `bin` folder. Assuming that the module `use.own` is loaded, then the contents of `modulesfiles` should be moved to `$HOME/privatemodules`

The `bin` directory contains the `texat` and `python` executables, and the `scripts` executables use these to perform various functions. The `texat-runtime.sif` image must be on `PATH`

## Usage
This directory contains the runtime scripts required to launch a TexAT Dask cluster on SLURM. It contains a few BlueBEAR specific variables, but otherwise targets SLURM.

To launch a cluster, first start the scheduler:
```bash
sbatch -t "24:00:00" launch-scheduler
```
The workers locate the scheduler using a configuration file that is written by the scheduler to `/mnt/backend` in the container. 

The `print-ssh-command` script will load this file and print the appropriate SSH command to tunnel into the scheduler node.

Then launch some single process workers:
```bash
sbatch -t 5:00:00 --array 0-395 --mem-per-cpu "1G" launch-worker
```

# texat-slurm
