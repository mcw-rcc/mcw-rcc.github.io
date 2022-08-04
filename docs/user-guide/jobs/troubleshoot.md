# Troubleshoot Jobs

It is often the case that your job will fail, or not do what you intended. This guide will show you how to proactively monitor your job and diagnose issues both in real-time and retrospectively. Depending on the platform you use to access the cluster, there are a variety of options to monitor and diagnose job issues. We suggest that you familiarize yourself with all of these resources.

## Command-line

The command-line has many powerful tools to monitor your jobs and diagnose issues. The first tool is the `squeue` command, which prints the current set of jobs in the queue. Monitoring the queue is best practice after you submit any job. It will quickly tell you if your job is running, where it is running, and for how long. When the cluster is busy, or you violate a scheduler policy, your job may be stuck in the queue. This will tell you the status of your job(s), and give reasons for any related issues.

To list only your jobs:
```bash
$ squeue -u NetID
```

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

### Accessing Compute Node

You may want to access the compute that is running your job to see further information in real-time. Direct compute node access via SSH is prohibited. If you need to access a compute node command-line during your job, you should run the job interactively. Please see [interactive jobs](running-jobs.md#submit-an-interactive-job) for details.

While direct SSH to a compute node is prohibited, there are other ways to pull real-time diagnostics from the compute node(s) that are running your job. For instance, you can run an additional command within an already running job with the `srun` command.

Suppose that we already have a running job on the cluster. 

``` bash
$ squeue -u NetID
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            263052    normal  testing    NetID  R       0:11      1 cn60
```

We can retrieve information, in this case hostname, from the compute node via `srun`.

``` bash
$ srun --jobid 263052 hostname
cn60.cluster.local
```

For useful diagnostics about running processes, we run the `ps u -u $USER` command.

``` bash
$ srun --jobid 263052 ps u -u $USER
USER           PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
username     67486  0.0  0.0 126484  2836 pts/0    Ss+  12:31   0:00 /usr/bin/bash
username     73572  0.0  0.0 165776  1932 ?        R    12:37   0:00 /usr/bin/ps u -u username
```

Passing inline commands to `srun` is limited. However, you can pass a script to print more information.

## Web portal (XDMoD)

XDMoD collects job accounting data and node level metrics during all cluster jobs. This data can be used for troubleshooting in the event of a crashed job.

Please see the [XDMoD guide](xdmod.md) for more info.