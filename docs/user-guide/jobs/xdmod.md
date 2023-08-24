# XDMoD

## Overview

XDMoD (XSEDE Metrics on Demand) is a NSF-funded open source tool that provides a wide range of metrics including utilization and performance of HPC resources. This tool can be used to monitor and troubleshoot your HPC jobs. It can also be used to track and document your RCC utilization. Please note that XDMoD uses your MCW login credentials. If you have not run jobs on RCC's HPC systems, you will not see any data after login. Please contact {{ support_email }} with questions.

!!! warning "Metrics are not real-time"
    XDMoD processes data overnight, rather than in real-time. This allows the application to be very responsive when displaying data. However, this means that **job data will not display in XDMoD until the following day**. Processing occurs every day at midnight.

## Login

Access RCC's XDMoD site at <https://xdmod.rcc.mcw.edu>. Click **Sign in** in the upper left corner.

![XDMoD Signin](../../_static/img/Xdmod_signin.png){ width="600" }

A login window will appear. Click the MCW logo.

![XDMoD Signin](../../_static/img/Xdmod_signin2.png){ width="300" }

You will be redirected to a login page. Enter your MCW credentials.

![XDMoD Signin](../../_static/img/Xdmod_signin3.png){ width="400" }

## User Guide

Please see the [XDMoD User Guide](https://xdmod.rcc.mcw.edu/user_manual/) for details on site features and navigation.

## Example Use - Troubleshooting a Job

XDMoD collects job accounting data and node level metrics during all cluster jobs. This data can be used for troubleshooting in the event of a crashed job.

To view job information in XDMoD:

1. Select the **Job Viewer** tab and follow the on screen instructions for job lookup. Once you have selected and saved the job(s) of interest, new folders with the resource and job id appear in the **Search History** on the left.
2. Select a job id to display job data. The Accounting data includes all information that XDMoD has gathered from the queuing system including job name, username, wait time, wall time, etc.
3. The Executable information will show the job's node name and application if available.
4. If your job shared the node with another job, the Peers tab will show the concurrent jobs (useful when memory issues occur).
5. Summary metrics include the processor, disk, and network usage. Detailed metrics contain all available node level metrics from your job.

--8<-- "includes/abbreviations.md"
