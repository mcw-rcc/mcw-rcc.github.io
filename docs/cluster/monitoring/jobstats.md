# Jobstats

Jobstats measures how efficiently your jobs use CPU, memory, and GPU resources. RCC uses the same data to power Job Defense Shield, which emails you (and sometimes cancels jobs) when a job is significantly under-using what it requested. This isn't about penalizing anyone — it's about freeing up idle resources for other researchers and helping your own jobs schedule faster by requesting only what you need.

Jobstats and Job Defense Shield were created by Princeton Research Computing.

## Running Jobstats

You do not need to wait for an alert to check your own jobs. Anyone can run JobStats at any time on any of their own job IDs:

```bash
jobstats <jobid>
```

Example output:

```txt
================================================================================
                              Slurm Job Statistics
================================================================================
         Job ID: 7654321
   User/Account: someuser/someaccount
       Job Name: samtools
          State: RUNNING
          Nodes: 1
      CPU Cores: 12
     CPU Memory: 90GB (7.5GB per CPU-core)
           GPUs: 1
  QOS/Partition: normal/gpu
        Cluster: hpc2020
     Start Time: Tue Sep 22, 2026 at 2:07 PM
       Run Time: 21:49:20 (in progress)
     Time Limit: 5-20:00:00

                         Overall Utilization
================================================================================
  CPU utilization  [|||||||||||||||||||||||||||||||||||||||||||||||95%]
  CPU memory usage [                                                1%]
  GPU utilization  [||||||||||||                                   25%]
  GPU memory usage [|                                               2%]
```

You can also view a graphical dashboard for your job — with CPU, memory, GPU, and runtime utilization plotted over time — through Open OnDemand.

[Jobstats Dashboard](https://ondemand.rcc.mcw.edu/pun/sys/jobstats){ .md-button .md-button--primary target="_blank" rel="noopener" }

## Job Defense Shield

Job Defense Shield runs on a schedule, reads job utilization data from Jobstats, and compares it against threshold policies. Depending on the alert type, it will warn by email and potentially cancel jobs that appear to be under-utilizing allocated resources. Every alert email includes the exact `jobstats` command and dashboard link for the affected job(s).

A common root cause behind several of the alerts below is requesting a resource for a job cannot use it. For example, request a GPU when your code only runs on CPU. Before submitting a job, please review the  [Submitting SLURM Jobs](../jobs/running-jobs.md) guide.

### Zero GPU Utilization Hours

Sent when your jobs have accumulated a significant number of GPU-hours at or near **0%** utilization.

**Common causes:**

- The application was not configured to use the allocated GPU(s) at all — for example, CPU-only code was run inside a GPU job allocation.
- Required GPU libraries or modules were not loaded.
- The application errored out immediately after starting, before reaching any GPU code.

**What to do:**

- Review the listed jobs with `jobstats <jobid>` for each one.
- Confirm your application is actually calling GPU code — check the software's own documentation for the correct flags/modules.
- Please investigate before submitting additional GPU jobs with the same configuration — repeated 0%-utilization GPU-hours reduce GPU availability for other researchers.

### Low GPU Efficiency

Sent when your jobs have accumulated a significant number of GPU-hours at or below the cluster average **50%** utilization

**Common causes:**

- The GPU is frequently waiting on CPU-side processing (a CPU bottleneck feeding the GPU).
- The workload/batch size is too small to keep the GPU busy.
- The application has configuration options controlling GPU utilization that haven't been tuned.
- An interactive session was allocated but spent significant time idle between uses.

**What to do:**

- Review affected jobs with `jobstats <jobid>` or the graphical dashboard to see the utilization pattern over time.
- Check your application's documentation for batch-size, worker-count, or data-loader settings that affect GPU utilization.
- Consider whether your job needs a full GPU allocation, or whether a shorter/smaller run would achieve the same result more efficiently.

### Excess CPU Memory Requested

Sent when you run a job requesting more CPU memory than required.

**Common causes:**

- `--mem` or `--mem-per-cpu` was set from a rough guess or copied from another job template rather than based on actual measured usage.
- The application's memory needs were overestimated for the input size actually being processed.

**What to do:**

- Check actual memory usage with `jobstats <jobid>` — look at the "CPU memory usage" line.
- A good target is using 80% or more of what you request. Lower your `--mem` or `--mem-per-cpu` directive for future jobs accordingly.
- Right-sizing memory requests also lets your jobs schedule faster.

### Low CPU Efficiency

Sent when you run a job requesting more CPU cores than required.

**Common causes:**

- The application is single-threaded or uses fewer threads/processes than the number of CPU cores requested with `--cpus-per-task` or `--ntasks`.
- Resource requests were based on a worst-case estimate rather than actual measured needs.
- The job spends significant time idle or waiting on I/O, another process, or a dependency.
- An interactive session was allocated but left idle for long stretches.

**What to do:**

- Review the job with `jobstats <jobid>` to see the actual CPU core and memory usage pattern.
- Match `--cpus-per-task`/`--ntasks` to what your application can actually use in parallel — check the application's own documentation for its threading/parallelism model.
- See [Submitting SLURM Jobs](../jobs/running-jobs.md) for guidance on requesting resources efficiently.

### Zero CPU Utilization

Sent when your jobs have accumulated a significant number of CPU-hours at or near **0%** utilization.

**Common causes:**

- The job or a launched process crashed or hung immediately without producing an error.
- A multi-node job failed to correctly launch worker processes on every allocated node.
- The application was waiting on a resource, license server, or dependency that never became available.

**What to do:**

- Check the job's actual output/log files for errors, not just its scheduler state.
- Use `jobstats <jobid>` to confirm which node(s) show 0% and cross-reference with your application logs for that node.
- Please investigate before submitting additional jobs with the same script/configuration.

### Excessive time limit requested

Sent when jobs are requesting far more walltime (`--time`) than required.

**Common causes:**

- `--time` was set to a large "safe" value (e.g., the partition maximum) out of caution, without being revisited once actual run times were known.
- A batch script or workflow template was copied from a much larger job.

**What to do:**

- Check actual run time vs. requested time limit with `jobstats <jobid>` (the report includes both "Run Time" and "Time Limit").
- Set `--time` closer to your job's actual typical run time, with a reasonable buffer — this usually improves scheduling for your jobs as well as others.

## Getting help

Replying to any Job Defense Shield email will automatically open a support ticket. If you believe an alert or cancellation happened in error, please reply with additional detail. You can also reach us directly at { help_email }.

If you receive an alert and aren't sure what it means for your specific job, or you'd like help improving resource utilization, **just reply to the alert email** — this automatically opens a support ticket with RCC.

See also:

- [Submitting SLURM Jobs](../jobs/running-jobs.md) — partitions, GRES/GPU syntax, and example batch
  scripts
- [User Etiquette](../etiquette.md) — general cluster usage expectations
- [Troubleshoot Jobs](../jobs/troubleshoot.md) — diagnosing failed or misbehaving jobs
