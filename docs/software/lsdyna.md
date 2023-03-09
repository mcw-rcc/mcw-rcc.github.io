# LS-DYNA

LS-DYNA is a general purpose finite element analysis software.

!!! info "Licensed Software"
    LS-DYNA is resticted to licensed users. Please contact {{ support_email }} with questions.

## Running LS-DYNA

LS-DYNA is installed on the HPC Cluster. RCC has provided the `sbatch-dyna` script which will build your submission file and submit it to the cluster.

!!! warning "sbatch-dyna required"
    You must use this script to submit LS-DYNA jobs on the cluster. Failure to do so will break the LS-DYNA licensing mechanism and result in failed jobs for users.

```txt
$ sbatch-dyna -h
Usage: sbatch-dyna [-i:-j:-m:-n:-c:-w:-q:-l:-p:-s:-y]
-i InputFile  (Enter input file, i.e. TestRun.k)
-j JobName    (Enter job name, i.e. TestRun)
-m Email      (Enter E-Mail for notification, i.e. user@mcw.edu)
-n Nodes      (Number of nodes, default=1)
-c Cores      (Number of cores, default=24)
-w Walltime   (Walltime, format HH:MM:SS, default=48:00:00)
-q Queue      (Queue, default=)
-l Module     (Choose module, 0 for lsdyna/9.0.1, 1 for lsdyna/9.3.1, 2 for lsdyna/10.2.0, 3 for lsdyna/11.2.0, 4 for lsdyna/12.0.0, 5 for lsdyna/8.0.0, 6 for lsdyna/8.1.0)
-p Precision  (Choose precision, i.e. [s]ingle or [d]ouble)
-s Submit     (Enter 'false' to create pbs script. Default true)
-y DynaArgs   (LS-Dyna args, i.e. -y memory1=200m memory2=100m, etc...)
-h, --help    show this help message and exit
```

Please note that `InputFile`, `JobName`, and `DynaArgs` are required by the script.

## Memory Allocation

It is very important to understand memory usage within LS-DYNA and how that corresponds to memory on a cluster node.

When submitting a job, there are 3 memory values to keep in mind:

- SLURM job memory limit
: The standard compute node max memory is 360GB. Do not exceed the memory limit.
- LS-DYNA memory1
: This is the amount of memory dedicated to the master process to decompose the model. This value is specified in mega-words (m) and must be less than or equal to the SLURM job memory limit.
- LS-DYNA memory2
: This is the amount of memory used by all CPU cores to solve the decomposed problem. This value is specified in mega-words (m) and must be less than or equal to the SLURM job memory limit divided by the number of cores used in the simulation.

## CPU Allocation

Matching a simulation to the correct node is important for both CPU use and memory allocation.

The HPC Cluster has nodes with the following CPU core counts:

- Standard compute node - 48 cores

## Example

Simulation requiring 1 node, 48 cores, and 360GiB. The HPC cluster automatically assigns 7.5GiB memory per core by default.

```txt
sbatch-dyna -i testing.k -j testrun -m testuser@mcw.edu -n 1 -c 48 -l 2 -p d -y "memory1=48240m memory2=1005m"
```

- `-n` flag is used to request 1 node
- `-c`flag is used to request 48 cores
- `memory1` is set to 48240m
: 1GiB is approximately 134m. Total memory is (134m * 360GiB = 48240m).
- `memory2` is set to 1005m
: Divide the total memory request by number of cores (48240m / 48 cores = 1005m).

## Getting Help

For more information about LS-DYNA memory settings please see <https://www.d3view.com/2006/10/a-few-words-on-memory-settings-in-ls-dyna/>.

If you have questions/concerns please email {{ support_email }}.
