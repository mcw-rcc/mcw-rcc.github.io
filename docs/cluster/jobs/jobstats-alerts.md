# JobStats and Automated Job Monitoring Alerts

This page explains **JobStats** — the tool RCC uses to measure how efficiently your jobs use
CPU, memory, and GPU resources — and documents each of the automated email alerts you may
receive from **Job Defense Shield**, our automated job-monitoring system. If you landed here
from a link in an automated email, jump directly to the section matching your alert using the
links in that email, or use the table of contents below.

## What is JobStats?

JobStats is a job-efficiency reporting tool (originally developed at Princeton University) that
RCC runs on top of Slurm and Prometheus/GPU-exporter metrics. It reports, per job:

- CPU utilization (percent of allocated CPU-core time actually used)
- CPU memory usage (used vs. allocated)
- GPU utilization (percent of allocated GPU time actually used)
- GPU memory usage (used vs. allocated)
- A time-series breakdown per node, and per GPU

RCC uses this same data to power **Job Defense Shield**, our automated monitoring system that
emails users (and in some cases automatically cancels jobs) when a job is significantly
under-utilizing the resources it requested. The goal of these alerts is not to penalize you —
it's to help free up CPUs, memory, and GPUs that are sitting idle so other researchers can use
them, and to help you request resources more accurately so your own jobs schedule faster in the
future.

### Running JobStats yourself

You do not need to wait for an alert to check your own jobs. Anyone can run JobStats at any time
on any of their own job IDs:

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

Useful flags:

| Flag | Purpose |
| --- | --- |
| `-j`, `--json` | Machine-readable output, no summary text |
| `-b`, `--base64` | JSON output, gzip + base64 encoded |
| `-f`, `--force` | Bypass cache and recalculate live |
| `-B`, `--batch-script` | Show the submitted Slurm batch script |
| `-n`, `--no-color` | Plain text output (useful when piping to a file) |

You can also view a **graphical dashboard** for any job — with CPU, memory, GPU, and runtime
utilization plotted over time — through Open OnDemand:

[Open the JobStats dashboard](https://ondemand.rcc.mcw.edu/pun/sys/jobstats){:target="_blank"}

## Choosing CPU vs. GPU resources for your job

A common root cause behind several of the alerts below is requesting a GPU allocation for a job
that never actually calls GPU code, or requesting far more CPU cores/memory than the application
uses. Before submitting a job, consider:

- **Does your application actually support GPU acceleration?** Not every build of every tool
  does. Check the application's own documentation for GPU support and required libraries/modules
  before requesting `--gres=gpu:N`. Loading the GPU-enabled module (where one exists) is often
  required — the GPU is not used automatically just because one was allocated.
- **Match your CPU-core and memory request to what the application needs**, not to the largest
  available node. Over-requesting cores or memory doesn't make your job faster, and it reduces
  the resources available to everyone else, which also slows down how quickly your own future
  jobs get scheduled.
- **Test at small scale first.** Run a short test job and check it with `jobstats` before
  scaling up to a long production run — it's much cheaper to discover a configuration problem in
  a 10-minute test job than after your job has run for 3 days at 0% GPU utilization.

See [Submitting SLURM Jobs](../running-jobs/) for partition names, GRES syntax, and example
batch scripts for both CPU and GPU jobs on the cluster.

## About Job Defense Shield

Job Defense Shield is RCC's automated job-monitoring system. It runs on a schedule, reads the
same utilization data as the `jobstats` command, and compares it against thresholds RCC has
configured for the cluster. Depending on the alert type, it will:

- **Warn you by email** that a job appears to be under-utilizing its allocated resources, with
  guidance on how to investigate and fix it, or
- **Automatically cancel a job** that has shown effectively zero GPU utilization for a sustained
  period, after first sending one or more warning emails.

Every alert email includes a JobStats command and a link to the graphical dashboard for the
specific job(s) affected, so you can see exactly what triggered the alert.

**Replying to any Job Defense Shield email automatically opens a support ticket with RCC** — if
you believe an alert or cancellation happened in error, or you'd like help improving a job's
resource usage, just reply.

---

## Automated GPU Utilization Monitoring and Cancellation {: #automated-gpu-utilization-monitoring }

RCC automatically monitors GPU jobs for utilization and will cancel jobs that show little or no
GPU activity for a sustained period. This protects a scarce, shared resource — idle GPUs held by
one job cannot be used by anyone else's job in the meantime.

**How it works:**

1. Every 15 minutes, running GPU jobs are sampled for GPU utilization.
2. If a job's GPU utilization stays below the utilization threshold, you'll receive a **first
   warning** email once the job has run about 1 hour with low activity.
3. If activity is still low, a **second warning** email follows roughly 45 minutes after the
   first.
4. If GPU activity has still not picked up by about 2 hours of low activity, **the job is
   automatically cancelled** (`scancel`) and you'll receive a cancellation notice.
5. Separately, a **sliding-window check** looks for jobs that go quiet for an extended stretch
   later in a long-running job (after 4 hours of inactivity a warning is sent; after 5 hours the
   job is cancelled), even if the job looked fine earlier on.

**Common causes of a cancellation or warning under this alert:**

- The application was launched without the flag(s) needed to actually use the GPU (many tools
  require an explicit `--gpu`, `--device=cuda`, or similar option, or a GPU-enabled module to be
  loaded).
- The job failed or hung early and never reached the GPU-computation stage, but kept running.
- The workload legitimately has long CPU-only phases (e.g., data loading, preprocessing) that can
  look like an idle GPU — if this is expected for your workflow, reply to the alert email and
  RCC can discuss an exclusion.
- An interactive session (Jupyter, RStudio, etc.) was left open and idle after you stopped
  actively using the GPU.

**What to do:**

- Check the job with `jobstats <jobid>` (see above) as soon as you get the first warning.
- If the job is no longer needed, cancel it yourself with `scancel <jobid>`.
- If you believe the GPU usage is legitimate but not being reported correctly, reply to the
  warning email to open a ticket before the cancellation deadline.

---

## Zero GPU Utilization Hours {: #zero-gpu-utilization-hours }

Sent when your jobs have accumulated a significant number of GPU-hours at or near **0%**
utilization over a recent lookback period. Unlike the real-time cancellation alert above, this is
a periodic summary — it looks back across multiple jobs and reports on the pattern.

**Common causes:**

- The application was not configured to use the allocated GPU(s) at all — for example, CPU-only
  code was run inside a GPU job allocation.
- Required GPU libraries or modules were not loaded.
- The application errored out immediately after starting, before reaching any GPU code.

**What to do:**

- Review the listed jobs with `jobstats <jobid>` for each one.
- Confirm your application is actually calling GPU code — check the software's own documentation
  for the correct flags/modules.
- Please investigate before submitting additional GPU jobs with the same configuration — repeated
  0%-utilization GPU-hours reduce GPU availability for other researchers.

---

## Low GPU Efficiency {: #low-gpu-efficiency }

Sent when your jobs are using the GPU, but at a mean utilization well below the cluster average
(RCC's current target is 50% or higher), across a meaningful number of GPU-hours.

**Common causes:**

- The GPU is frequently waiting on CPU-side processing (a CPU bottleneck feeding the GPU).
- The workload/batch size is too small to keep the GPU busy.
- The application has configuration options controlling GPU utilization that haven't been tuned.
- An interactive session was allocated but spent significant time idle between uses.

**What to do:**

- Review affected jobs with `jobstats <jobid>` or the graphical dashboard to see the utilization
  pattern over time.
- Check your application's documentation for batch-size, worker-count, or data-loader settings
  that affect GPU utilization.
- Consider whether your job needs a full GPU allocation, or whether a shorter/smaller run would
  achieve the same result more efficiently.

---

## Excess CPU Memory Requested {: #excess-cpu-memory }

Sent when your jobs are requesting substantially more CPU memory than they actually use — this
alert only fires when CPU efficiency is also low, which usually means the resource request
overall (cores and memory) was larger than the job needed.

**Common causes:**

- `--mem` or `--mem-per-cpu` was set from a rough guess or copied from another job template
  rather than based on actual measured usage.
- The application's memory needs were overestimated for the input size actually being processed.

**What to do:**

- Check actual memory usage with `jobstats <jobid>` — look at the "CPU memory usage" line.
- A good target is using 80% or more of what you request. Lower your `--mem` or `--mem-per-cpu`
  directive for future jobs accordingly.
- Right-sizing memory requests also lets your jobs schedule faster, since large memory requests
  can be harder for Slurm to place.

---

## Low CPU Efficiency {: #low-cpu-efficiency }

Sent when your jobs show both low CPU utilization **and** low memory utilization over a
significant number of CPU-hours — a strong sign that more cores (and memory) were requested than
the application is actually using.

**Common causes:**

- The application is single-threaded or uses fewer threads/processes than the number of CPU
  cores requested with `--cpus-per-task` or `--ntasks`.
- Resource requests were based on a worst-case estimate rather than actual measured needs.
- The job spends significant time idle or waiting on I/O, another process, or a dependency.
- An interactive session was allocated but left idle for long stretches.

**What to do:**

- Review the job with `jobstats <jobid>` to see the actual CPU core and memory usage pattern.
- Match `--cpus-per-task`/`--ntasks` to what your application can actually use in parallel — check
  the application's own documentation for its threading/parallelism model.
- See [Submitting SLURM Jobs](../running-jobs/) for guidance on requesting resources
  efficiently.

---

## Zero CPU Utilization {: #zero-cpu-utilization }

Sent when one or more nodes allocated to your job showed **0%** CPU utilization for a meaningful
stretch of runtime — the job appears to be running, but nothing on that node is actually doing
work.

**Common causes:**

- The job or a launched process crashed or hung immediately without producing an error, while the
  Slurm allocation continued running.
- A multi-node job failed to correctly launch worker processes on every allocated node.
- The application was waiting on a resource, license server, or dependency that never became
  available.

**What to do:**

- Check the job's actual output/log files for errors, not just its Slurm state.
- Use `jobstats <jobid>` to confirm which node(s) show 0% and cross-reference with your
  application logs for that node.
- Please investigate before submitting additional jobs with the same script/configuration.

---

## Excessive Time Limit Requested {: #excessive-time-limit }

Sent (to RCC staff, for awareness — not typically to the submitting user as an urgent action
item) when jobs are requesting far more walltime (`--time`) than they actually use, across a
large number of cumulative hours. Overestimating walltime makes jobs harder for Slurm to
schedule around and can inflate apparent queue congestion.

**Common causes:**

- `--time` was set to a large "safe" value (e.g., the partition maximum) out of caution, without
  being revisited once actual run times were known.
- A batch script or workflow template was copied from a much larger job.

**What to do:**

- Check actual run time vs. requested time limit with `jobstats <jobid>` (the report includes
  both "Run Time" and "Time Limit").
- Set `--time` closer to your job's actual typical run time, with a reasonable buffer — this
  usually improves scheduling for your own subsequent jobs as well as others'.

---

## Getting help

If you receive an alert and aren't sure what it means for your specific job, or you'd like help
improving resource utilization, **just reply to the alert email** — this automatically opens a
support ticket with RCC. You can also reach us directly at
[help-rcc@mcw.edu](mailto:help-rcc@mcw.edu).

See also:

- [Submitting SLURM Jobs](../running-jobs/) — partitions, GRES/GPU syntax, and example batch
  scripts
- [User Etiquette](../../etiquette/) — general cluster usage expectations
- [Troubleshoot Jobs](../troubleshoot/) — diagnosing failed or misbehaving jobs
