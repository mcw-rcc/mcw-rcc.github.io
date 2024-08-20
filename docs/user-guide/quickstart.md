# Cluster Quick Start

This guide contains the minimal steps to get started running computational workflows, with links to further reading included. If you're a first time user, please follow the links and review the full documentation.

Need help getting started? Send us an email at {{ support_email }}.

## Getting an account

You need an RCC account to get started. Please see [Getting an Account](accounts.md) for details.

## Logging in

Login is available both on and off campus via SSH and Open OnDemand. Please see the [Login guide](access/login.md) for details.

!!! warning "Clusters are shared resources."
    Please be respectful of all other users. Do not start resource-intensive scripts on a login node. Do not request more resources than your job can use. For more info see [User Etiquette](etiquette.md).

## Cluster Storage

Your account has access to a set of storage directories by default. You can easily find your available storage paths and current utilization on the cluster with the `mydisks` command. Please see the [storage guide](../storage/rcc-storage.md) for details.

```txt
$ mydisks
=====My Lab=====
Size  Used Avail Use% File
47G   29G   19G  61% /home/user
932G  158G  774G  17% /group/pi
4.6T     0  4.6T   0% /scratch/g/pi
```

## Transferring Files

Most users will need to transfer files to cluster storage. Several methods are available depending on your need. These include Open OnDemand, SSH (command-line), and desktop client software. Please see the [file transfer guide](../storage/file-transfer.md) for details.

## Using Software

Software is managed by modules using [Lmod](https://lmod.readthedocs.io/en/latest/){:target="_blank"}. The modules contain information about an application's version, executable, libraries, and documentation. Using module commands, users can add, remove, or switch versions of a application. Please see the [cluster software guide](../software/modules.md) for more information.

## Running a SLURM Job

The clusters use SLURM to manage jobs. Most jobs are scheduled using batch job scripts. A batch job script includes a request for cluster resources, and the commands to run in the job. Each batch job is submitted by the user for remote execution on a compute node. The SLURM scheduler decides where the job should run, based on the requested and available resources.

Please see the [SLURM job guide](jobs/running-jobs.md) for details on how to write, submit and monitor a job script.

## More info

This guide has links to additional information about each topic. We strongly recommend to review all documentation.

Need help getting started? Send us an email at {{ support_email }}.

--8<-- "includes/abbreviations.md"
