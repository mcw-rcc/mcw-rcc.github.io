# Troubleshoot Jobs

It is often the case that your job will fail, or not do what you intended, or stay queued longer than usual. This guide will show you how to proactively monitor your job and diagnose issues both in real-time and retrospectively. Depending on the platform you use to access the cluster, there are a variety of options to monitor and diagnose job issues. We suggest that you familiarize yourself with all of these resources.

## Command-line

### Queued or Running jobs

The command-line has many powerful tools to monitor your jobs and diagnose issues. The first tool is the `squeue` command, which prints the current set of jobs in the queue. Monitoring the queue is best practice after you submit any job. It will quickly tell you if your job is running, where it is running, and for how long. When the cluster is busy, or you violate a scheduler policy, your job may be stuck in the queue. This will tell you the status of your job(s), and give reasons for any related issues.

To list only your jobs:

```bash
$ squeue -u NetID
JOBID   PARTITION   NAME        USER    ST  TIME    NODELIST(REASON)
263052  normal      testing     NetID   R   0:11    cn60
348944  sys/dash    hello       NetID   R   5:10    cn58
789101  sys/dash    my_test     NetID   PD  0:00    (QOSMaxJobsPerUserLimit)
112131  normal      test        NetID   PD  0:00    (Resources)
123456  normal      other_test  NetID   PD  0:00    (Dependency)
```

The last column of the `squeue` command will print the list of allocated nodes for running jobs and the reason why the job has not started in parenthesis for pending jobs. These are some of the common reasons you will find:

- `QOSMaxJobsPerUserLimit`: This code indicates that you reached the number of running jobs allowed per user for the corresponding partition. In this case, the job must wait until other job running in that partition completes. This often happens when a user tries to run more than one interactive job (which includes OnDemand interactive jobs). You are only allowed to run one interactive job at a time. These jobs have the value `sys/dash` in the `PARTITION` column.
- `Resources`: This code indicates that the job is waiting for resources to be available. Make sure that you don't request more resources than what you need. The more resources you request, the higher chance that some of them might not be available.
- `Dependency`: This code will appear if a job dependency is not yet satisfied. You can learn more about job dependencies in our video for [creating an advanced SLURM script](https://www.youtube.com/watch?v=-4mBhe5cK7o&t=1452s){:target="_blank"}.
- `Maintenance`: If you submitted your job close to the maintenance window and it wouldn't be able to finish (according to the requested wall time) before maintenance starts, it will be paused until maintenance is over. When you log to the RCC through ssh, you will see some messages. Log in and out and check if an upcoming maintenance is starting soon.

You can obtain detailed information about a queued or running job using the following command, including more detailed information why a job is pending or a list of allocated resources for running jobs:

```bash
scontrol show job jobID
```

`sacct` also provides extended information about a running or pending job:

```bash
sacct -j jobID --format=list_of_columns
```

To see the list of columns that can be printed you can run the command with the following flag:

```bash
sacct --helpformat
```

In the following example we can see the time when job `5198144` was submitted, when it became eligible to run, when it started running, and how much time has passed since then. `head -n 3` limits the number of lines printed to those of interest:

```bash
$ sacct -j 5198144 --format=JobID,JobName,Submit,Eligible,Start,Elapsed | head –n 3
JobID    JobName  Submit              Eligible            Start               Elapsed
-------- -------- ------------------- ------------------- ------------------- ----------
5198144  namd-sf  2025-04-18T16:08:39 2025-04-18T16:08:39 2025-04-18T16:08:48 3-22:18:57
```

You can use `sinfo` to view information about the partition in which you submitted your job. This can provide an insight on how busy that partition is, which might explain why your job is taking long to run. In the example below you can look at the normal partition. 55 of the 60 nodes have all their CPUs allocated, 2 of them are idle, and 3 of them are on a mix state, which means that they have some of their CPUs allocated while others are idle. In this case, the cluster is very busy, and it would be normal for jobs to take long to start running. To prevent a state like this from happening, users should be mindful to only request those resources that they are going to use:

```bash
$ info
PARTITION   AVAIL   TIMELIMIT   NODES  STATE    NODELIST
normal*     up      7-00:00:00  3      mix      cn[12,32-33]
normal*     up      7-00:00:00  55     alloc    cn[01-11,13-31,36-60]
normal*     up      7-00:00:00  2      idle     cn[34-35]
bigmem      up      7-00:00:00  2      mix      hm[01-02]
gpu         up      7-00:00:00  2      mix      gn[01,04]
gpu         up      7-00:00:00  7      alloc    gn[02-03,05-09]
```

You can investigate further the mix nodes to see how busy they really are. In the following example we are looking at the number of CPUs and memory allocated in compute node 12 (marked as `mix`). Compute nodes have 48 cores per node. In this node, 32 of those 48 cores are allocated. So, the node is indeed very busy. You can learn more about the number of cores and memory that nodes in each partition have in our [hardware documentation](https://docs.rcc.mcw.edu/cluster/hardware/#cluster).

```bash
# View allocated resources on node cn12
$ scontrol show node cn12 | grep AllocTRES
AllocTRES=cpu=32,mem=18G 
# View all jobs running on cn12
$ squeue | grep cn12
```

Your kobs wait time will be affected by their priority, which is managed by the [fairshare scheduler algorithm](https://slurm.schedmd.com/fair_tree.html){:target="_blank"}. It is based on the cluster utilization from the previous 7 days, to promote equal use of the cluster. So, when a user increases their cluster utilization, their job priority is lowered compared to infrequent users. `sprio` is used on pending jobs. In the following example we run it twice. As you can see, the priority and age will change with time. Jobs with higher priority will run first. `AGE` is the length of time that your job is been seated in queue. `PRIORITY` increases as `AGE` increases to help ensure that jobs don't get stuck indefinitely in queue. `FAIRSHARE` is a measure of how much a user has been using the cluster over the past week. `PRIORITY` increases as `FAIRSHARE` decreases to prevent overuse of resources. `JOBSIZE` is the number of cores requested. `PRIORITY` increases with `JOBSIZE` to avoid larger jobs sitting in queue for too long. These relationships are not lienar and will consider all other factors.

```bash
$ sprio –j 5197367
JOBID   PARTITION  PRIORITY  SITE  AGE  FAIRSHARE  JOBSIZE  QOS TRES
5197367 gpu        1372      0     0    8432       225      0   cpu=83,mem132,gres/
$ sprio –j 5197367
JOBID   PARTITION  PRIORITY  SITE  AGE  FAIRSHARE  JOBSIZE  QOS TRES
5197367 gpu        1374      0     2    8432       225      0   cpu=83,mem132,gres/
```

You can use `sprio` to check how many jobs are ahead of yours in the corresponding partition queue. Replace `gpu` by your job partition and `5197367` by your jobID. The output will highlight your jobID. In this example, there are 6 jobs ahead of `5197367`.

```bash
$ sprio –p gpu --sort –y | awk '{print NR-1 $0}' | less +g -p 5197367
0 JOBID PARTITION PRIORITY SITE AGE FAIRSHARE JOBSIZE  QOS  TRES
1 5200677 gpu     9835     0    0   7564      1353     0    cpu=118,mem=176,gres
2 5191312 gpu     2239     0    6   111       1353     0    cpu=142,mem=3,gres/g
3 5191332 gpu     2239     0    6   111       1353     0    cpu=142,mem=3,gres/g
4 5168126 gpu     2239     0    6   111       1353     0    cpu=142,mem=3,gres/g
5 5191352 gpu     2236     0    3   111       1353     0    cpu=142,mem=3,gres/g
6 5167923 gpu     2236     0    3   111       1353     0    cpu=142,mem=3,gres/g
7 5197367 gpu     1376     0    27  222       64       0    cpu=142,mem=28,gres/
8 5198832 gpu     1377     0    27  222       64       0    cpu=142,mem=28,gres/
9 5198833 gpu     1107     0    27  222       64       0    cpu=142,mem=28,gres/
```

In the command above `-p gpu` selects only jobs in the `gpu` partition. `--sort -y` tells `sprio` to print jobs in descending priority, printing those with higher priority first. `awk '{print NR-1 $0}'` prints line numbers for easy visualization of how many jobs are ahead of our job of interest. `less +g -p 5197367` highlights the line that contains our jobID, `5197367`.

You can examine a running job using an interactive shell. This command will place your shell on the head node of the running job. From there, you can run commands like `top`. The `-i` flag will hide all idle processes, making it easier to visualize what is going on.

```bash
$ sbatch script.sh
Submitted batch job 5224589
$ srun --jobid=5224589 --pty bash
(job 5224589) $ top –i
```

The image below is an example of what the output of `top` looks like. In this example there are a couple of things showing that the node where our job is running is overloaded. First, the load average is 49, which is higher than the number of CPUs, which is 48 in all compute nodes. This means that in average, all cores are being used and some processes are waiting for CPU time. We can also see that at the moment `top` ran, 99.4 percent of the CPUs were being used and most processes are using 100% of the CPU. So, it is inevitable that some processes will be sleeping for some periods of time, waiting for available CPUs. We can from the 679 total tasks in this node, 50 are running and 629 are sleeping.

### Finished jobs

Another useful command is `seff`, which prints the workload efficiency metrics for a job. Although you can run this tool during a job, it is best used after your job is finished. The output will give an estimate of CPU and memory efficiency, and is very useful when benchmarking a workload for memory limit.

To list only your jobs:

```bash
$ seff 250
Job ID: 250
Cluster: cluster
User/Group: user/sg-group
State: COMPLETED (exit code 0)
Nodes: 1
Cores per node: 5
CPU Utilized: 00:00:01
CPU Efficiency: 6.67% of 00:00:15 core-walltime
Job Wall-clock time: 00:00:03
Memory Utilized: 0.00 MB (estimated maximum)
Memory Efficiency: 0.00% of 30.00 GB (30.00 GB/node)
```

If a job failed, the line that starts with `State:` will show why it failed. If it says `OUT OF MEMORY`, ju just need to increase the amount of memory that you request and re-submit the job. If it says `TIMEOUT`, you need to increase tje wall time. If you don't know how to modify the resources that you request for a job, we suggest that you watch our video on [how to write a simple SLURM script](https://www.youtube.com/watch?v=gIL7GOxCszw&t=10s){:target="_blank"}.

If the state is `FAILED`, most probably there's a bug in your code. You can obtain more information by looking at the log file. If the log file shows an error that includes `command not found`, it is because you forgot to load the corresponding module, or loaded the wrong module. If there's no useful information in the log file and none of the previous tools have help you resolve the issue, you can run an interactive job to debug your code. Use `srun` followed by the SBATCH options that you used in the submission script, followed by `--pty bash`. Then, you can execute line by line the code in a compute node and see where it fails. You can precede your commands with `time` to see the time that each command takes to execute:

```bash
$ srun --ntasks=1 --mem-per-cpu=4GB --time=01:00:00 --job-name=interactive --account=PI_NetID --pty bash
(job 5227687) $ module load fsl
(job 5227687) $ time fslstats HIE-21_T1.nii.gz -M
69.137432

real    0m0.167s
user    0m0.123s
sys     0m0.018s
```

Finally, if you run jobs in OnDemand often, eventually your home directory will get full of some temporary files that are created every time you start an OnDemand job. These files are hidden. So, it's easy to miss that your home directory is getting full. To check the available storage in your home directory, use `mydisks`. You will see how much space you have used and how much is available. If your home directory is full, your jobs will start failing. When you search the content of your home directory, include the `-lah` flag to include hidden files and display the size of files and directories in human-readable format. Search for a folder called `.cache`. It will be written and will increase in size every time you run an OnDemand job.

```bash
$ cd /home/myuser/
$ mydisks
=====My Lab=====
 Size  Used Avail Use% File
  47G   35G   12G  76% /home/myuser
$ ls -lah
drwx------. 57  myuser sg-mygroup  46K  May 1 11:00 .
drwxr-xr-x. 123 root   root        0      May 1 10:26 ..
-rw-------. 1   myuser sg-mygroup  23K  May 1 10:53 .bash_history
drwxr-xr-x. 278 myuser sg-mygroup  140K May 1 11:00 .cache
drwxrwxr-x. 37  myuser sg-mygroup  20K  May 1 11:00 .config
```

## Web portal (XDMoD)

XDMoD collects job accounting data and node level metrics during all cluster jobs. This data can be used for troubleshooting in the event of a crashed job.

Please see the [XDMoD guide](xdmod.md) for more info.
