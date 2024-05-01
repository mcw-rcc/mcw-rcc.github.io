# HPC Cluster Operating System Upgrade

!!! tip "Project complete"
    Research Computing completed this project on May 1, 2024. This page will remain available for a short time as a reference to users.

The HPC cluster operating system, CentOS 7, will be end-of-life in June 2024 and we decided to upgrade to Rocky Linux 8. Research Computing completed the upgrade on May 1, 2024.

## Potential issues

- System Python2 will no longer be installed. Python2 has been end-of-life and unsupported since 2020. If you have workflows built on Python2, upgrade the code to Python3. If you must use Python2, you may use the centrally installed modulefile `python/2.7.18`.
- System library versions will change. System libraries such as GCC will have newer versions in Rocky Linux 8. If you have compiled your workflow with system default libraries, you will need to rebuild.

## Timeline

The scheduled dates below are subject to change. Please check regularly for updates.

### User testing phase (completed)

***December 13, 2023 - April 3, 2024***

This phase is now closed.

### Upgrade and extended testing (completed)

***April 3, 2024 - {--June 5, 2024--} May 1, 2024***

A majority of login and cluster nodes are now running Rocky Linux 8. We will maintain a login node and small number of cluster nodes on the legacy CentOS 7 operating system for the duration of this phase. This allows backwards compatibility for users that did not test their workflows in the previous phase.

To access the legacy system:

```bash
ssh login-centos7.rcc.mcw.edu
```

We have legacy partitions (queues) setup to support workflows that do not work on Rocky Linux 8. To run a job on a job with legacy CentOS 7 on a CPU compute node, use `--partition=centos`. To run a job on a job with legacy CentOS 7 on a GPU compute node, use `--partition=centos-gpu`.

!!! warning "Legacy CentOS 7"
    The legacy queues exist to support workflows temporarily. If you did not test your workflows in the previous phase, we suggest you do that and then use the normal queues that now run on Rocky Linux 8. Do not use these queues as a permanent fix for not testing your workflows. If you find your workflow does not work on the upgraded cluster, please contact {{ support_email }} for assistance.

### Final upgrade (completed)

***{--June 5, 2024--} May 1, 2024***

The remaining login and cluster nodes will be upgraded to the new operating system on May 1, 2024. This will conclude the upgrade process.

## FAQ

??? question "Does the upgrade to Rocky Linux 8 affect reproducibility of my work?"
    Yes, its possible. A change in operating system means updated system libraries, which may affect the performance and output of your code.

??? question "Does this change the way I submit my jobs?"
    No, we did not change how you interact with SLURM. However, we did update SLURM (which we do regularly).

??? question "Did this change how I use OnDemand?"
    No, we did not change how you interact with OnDemand. However, we did update OnDemand (which we do regularly).
