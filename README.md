# TexAT Slurm Cluster
This repository contains the runtime scripts required to launch a TexAT Dask cluster on SLURM. It contains a few BlueBEAR specific variables, but otherwise targets SLURM.

## Setup
1. The `use.own` module should be loaded by default on the target system
2. Ansible should be installed `pip install -U ansible`
3. Run the setup playbook `ansible-playbook setup.yml --extra-vars texat_shared_path=<TEXAT_SHARED_PATH>`, where `<TEXAT_SHARED_PATH>` is the path to the shared storage

The `bin` directory contains the `texat` and `python` executables, and the `scripts` executables use these to perform various functions. The `texat-runtime.sif` image must be on `PATH`

## Usage
First, load the `texat` module:
`module load texat-1.0.0`.

To launch a cluster, first start the scheduler:
```bash
sbatch -t "24:00:00" launch-scheduler
```
The workers locate the scheduler using a configuration file that is written by the scheduler to `/mnt/data` in the container. 

The `print-ssh-command` script will load this file and print the appropriate SSH command to tunnel into the scheduler node.

Then launch some single process workers:
```bash
sbatch -t 5:00:00 --array 0-395 --mem-per-cpu "1G" launch-worker
```
