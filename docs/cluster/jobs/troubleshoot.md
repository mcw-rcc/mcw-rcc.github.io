# Troubleshoot Jobs

## Overview

It is often the case in computational work that your job may not do what you expected, or intended. This can be confusing and is a leading source of questions to Research Computing. This guide will explain more about queueing system outcomes and show you how to proactively diagnose the most common issues.

## Job is not running

If your job is queued longer than expected, you can find a reason with the `squeue -j jobId` command, where `jobId` is the SLURM job number.

```bash
$ squeue -j 12345
JOBID   PARTITION   NAME        USER    ST  TIME    NODELIST(REASON)
12345   normal      testing     NetID   PD  0:00    (QOSMaxJobsPerUserLimit)
```

In the example job **12345** the state (ST) is reported pending (PD). The reason is listed last under the **NODELIST(REASON)** heading (a compute node list is printed if job is running). The reason listed for the example job is **QOSMaxJobsPerUserLimit**. This translates to a queue (partition) based resource limit for users. It is important to know that the cluster has limits on what resources any single lab group or user can use. These [resource limits](running-jobs.md#resource-limits), along with other cluster policies, help maintain fair utilization of the cluster.

So, the example job is in the queue because of the cluster's resource limits. Several scenarios could result in this example. The most likely explanation is that you are already running jobs on the cluster, and you've hit that particular limit. In that case, the best thing to do is wait, and the job will likely start running when your previous jobs finish. However, there are other cases of resource limits that are less obvious. For example, the cluster does limit the number of interactive jobs you can run.

The reason listed for our example job is one of several that SLURM provides. Please see the table below for additional examples.
<!-- markdownlint-disable MD033 -->
| <div style="width:12em">Reason</div> | Why | What to do |
| ------ | --- | ---------- |
| `QOSMaxJobsPerUserLimit` | You reached the number of running jobs allowed per user for the corresponding partition. | You can wait for previous jobs to finish or cancel running jobs. |
| `Resources` | The job is waiting for resources to become available. | In many cases you should wait and the job will run. Additionally, try to make sure that you don't request more resources than what you need. |
| `Dependency` | A job dependency is not yet satisfied. | You can learn more about job dependencies in our video for [creating an advanced SLURM script](https://www.youtube.com/watch?v=-4mBhe5cK7o&t=1452s){:target="_blank"}. |
| `Maintenance` | Your job is blocked until maintenance is finished. | You can wait or cancel and resubmit your job according to the [job scheduling and maintenance](../jobs/running-jobs.md#job-scheduling-and-maintenance) section of our job submission guide. |
| `Priority` | Your job is waiting for higher priority jobs to finish. | Please wait and rest assured your job will run in due time. For more info about job priority, please see the [job priority section](../jobs/running-jobs.md#fairshare-and-priority) of our job submission guide. |
<!-- markdownlint-enable MD033 -->
!!! tip "When to contact help"
    It is not possible to provide an exhaustive table of scenarios, reasons, and guidance here. When in doubt, please feel free to contact {{ support_email }}.

## Job failed immediately

Many issues can cause a job to fail immediately. By immediately, we mean the job starts and finishes without producing any useful output. Often there is an error listed in the job output file. The output file is named according to your job name, or can have a specific syntax based on your `#SBATCH --output` option. Most often we find the job output file is something like `slurm-12345.out`, where **12345** is the jobId number.

Incorrect file names or paths are the most common source of immediate job failure. You will see a specific error in your output file with the syntax `command: /file/path: No such file or directory`. This indicates that your job is trying to manipulate a file or directory that does not exist. Most often this is a simple typo error, but could also be caused by trying to use files in `/group`, which is not available on compute nodes. We suggest to double check the file names and paths, and then resubmit. If the issue persists, contact {{ support_email }}.

Storage limits can also cause this issue. Every user has access to at least three storage paths including `/home/netId`, `/group/pi_netId`, and `/scratch/g/pi_netId`. Each of these spaces has a finite limit according to our [storage guide](../../storage/rcc-storage.md). Your job should be using `/scratch` for input/output and will fail immediately if your scratch quota is 100% full. You can find your available storage paths and quotas with the `mydisks` command.

```txt
$ mydisks
=====My Lab=====
Size  Used Avail Use% File
47G   29G   19G  61% /home/netId
932G  158G  774G  17% /group/pi_netId
4.6T     0  4.6T   0% /scratch/g/pi_netId
```

Finally, if you run jobs in OnDemand often, your home directory will fill with temporary files that are created every time you start an OnDemand job. If you primarily use OnDemand and your apps are failing to start, check your home directory limit with `mydisks`. If your home directory is full, look for files in `/home/netId/ondemand`, where `netId` is your username. You can safely clean out this folder and proceed with a logout/login on OnDemand.

## Job stopped unexpectedly

Jobs that start and run correctly can still stop or fail unexpectedly for a variety of reasons. Again, the output file is a good starting place and may contain a useful error for diagnosing the issue.

### Memory

A common failure is the job running out of memory. Jobs that fail with memory issues often produce normal, useful output until they stop abruptly. In this case, your job output file might reference an `OOM` or `Out-Of-Memory` error.

Another way to diagnose memory issues is the `seff` command.

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

If your job fails due to memory, the **State:** output will show **OUT_OF_MEMORY**. You will also see high memory utilization above what you allocated. To fix a memory issue, try to increase the amount of memory in the job script and re-submit.

### Walltime

Another common failure of running jobs is a job timeout. Again, you will see an abrupt end to an otherwise well running job. A job can timeout if it tries to run longer than your requested walltime or the max walltime. Walltime tells the cluster how long you expect the job to run, and has a max value of 7 days. You can also use the `seff` command to diagnose this issue. If a job fails due to timeout, the **State:** output will show **TIMEOUT**. To fix a walltime issue, try to increase the walltime or shorten the simulation, and resubmit.

## Other resources

Research Computing provides a web portal with all job information called XDMoD. XDMoD collects job accounting data and node level metrics during all cluster jobs. This data can be used for troubleshooting in the event of a failed job. However, XDMoD is only useful for retrospective analysis. It collects and aggregates data once per day, rather in realtime. Jobs that run and finish one day will be available in XDMoD the following day.

Please see the [XDMoD guide](../jobs/xdmod.md) for more info.

## Getting help

It is not possible to provide an exhaustive list of scenarios, reasons, and guidance here. When in doubt, please contact {{ support_email }} for expert help.
