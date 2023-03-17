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

- **SLURM job memory limit**  
The standard compute node max memory is 360GB. Do not exceed the memory limit. If you need additional memory, use the large memory nodes.
- **LS-DYNA memory1**  
This is the amount of memory dedicated to the master process to decompose the model. This value is specified in mega-words (m). A mega-word is 4 bytes in single precision, or 8 bytes in double precision. Memory1 is dependent on your model size.
- **LS-DYNA memory2**  
This is the amount of memory used by each CPU core to solve the decomposed problem. Memory2 is dependent on the number of cores requested. More cores means less memory required per core.

## CPU Allocation

Every cluster node has 48 cores. Please remember that more cores does not always mean faster.

## Example

Simulation using 1 node, 48 cores, and 360GB.

```txt
sbatch-dyna -i testing.k -j testrun -m netid@mcw.edu -n 1 -c 48 -l 2 -p d -y "memory1=16000m memory2=1005m"
```

- `memory1`  
We request 16000m (16,000 mega-word) for model decomposition, or 128GB in double precision.

$$
16000 \text{mega-word} \times \frac{1e^6 \text{word}}{1 \text{mega-word}} \times \frac{8 \text{byte}}{1 \text{word}} \times \frac{1\text{gigabyte}}{1e^9 \text{byte}} = 128\text{GB}
$$

- `memory2`  
We request 333m (333 mega-words) per CPU core, or ~2.67GB per CPU core. Total memory allocated to all CPU cores is ~128GB. The remaining 104GB is available for dynamic memory allocation.

$$
333 \text{mega-word} \times \frac{1e^6 \text{word}}{1 \text{mega-word}} \times \frac{8 \text{byte}}{1 \text{word}} \times \frac{1\text{gigabyte}}{1e^9 \text{byte}} = 2.67\text{GB}
$$

!!! tip "Additional memory guidance"
    The example above is just an example. Please adjust for the memory needs of your simulation. You may need more or less memory depending on size of model. You may also need to substitute `4 bytes` into the calculations if you use single precision. The process of determining memory utilization should be iterative. For more advice on LS-DYNA memory, see please see <https://www.d3view.com/a-few-words-on-memory-settings-in-ls-dyna/>.

## Getting Help

If you have questions about the `sbatch-dyna` command or the HPC cluster, please email {{ support_email }}. Please email the software vendor for LS-DYNA questions.
