# Maintenance

Research Computing uses scheduled maintenance windows to perform upgrades and general upkeep of all systems. While every effort will be made to adhere to the published schedule, dates and times may change.

## 2026 Schedule

- January 13, 2026 8AM-5PM
- April 14, 2026 8AM-5PM
- July 14, 2026 8AM-5PM
- October 13, 2026 8AM-5PM

## Job scheduling around maintenance

If you submit a job before a maintenance window and the job is sitting in the queue, first check to see why with `squeue`. In the output, you'll see the last column `NODELIST(REASON)`. If your job is held for maintenance, you'll see `(ReqNodeNotAvail, Reserved for maintenance)`.

Each job has a [walltime request](cluster/jobs/running-jobs.md#time). The walltime request sets the amount of time that your job will be allowed to run on the cluster. On the HPC cluster, the maximum allowed walltime is 7 days.

If you submit your job with X hours walltime request, and the maintenance window starts in less than X hours, the job will be held until after the maintenance window closes. The reason is that the scheduler cannot guarantee your job will finish before maintenance begins.

To make the job run, resubmit your job with a walltime request that ensures the job will finish before the maintenance window. We can use a simple formula to find that walltime.

`walltime = ( $maint_start_time - $current_time )`

To simplify the process, use the `maxwalltime` command, which provides the maximum walltime request that will also allow your job to run before maintenance.
