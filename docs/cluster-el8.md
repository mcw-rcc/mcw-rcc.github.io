# HPC Cluster Operating System Upgrade

The HPC cluster operating system, CentOS 7, will be end-of-life in June 2024 and we are beginning a project to upgrade. This will allow us to maintain ongoing security patching and compatibility with new software through the cluster lifetime. To that end, we have chosen to upgrade the HPC cluster to Rocky Linux 8.

We are currently working on a smooth upgrade path that will minimize downtime and disruption to workflow. This includes testing most centrally installed software packages for compatibility. When necessary, we will rebuild those software packages while maintaining version if possible. We do anticipate that certain workflows will be disrupted, especially for users that have installed their own software.

## Potential issues

- System Python2 will no longer be installed. Python2 has been end-of-life and unsupported since 2020. If you have workflows built on Python2, upgrade the code to Python3. If you must use Python2, you may use the centrally installed modulefile `python/2.7.18`.
- System library versions will change. System libraries such as GCC will have newer versions in Rocky Linux 8. If you have compiled your workflow with system default libraries, you will need to rebuild.

## Timeline

The scheduled dates below are subject to change. Please check regularly for updates.

### User testing phase (closed)

***December 13, 2023 - April 3, 2024***

This phase is now closed.

### Upgrade and extended testing (Active now)

***April 3, 2024 - June 5, 2024***

A majority of login and cluster nodes are now running Rocky Linux 8. We will maintain a login node and small number of cluster nodes on the legacy CentOS 7 operating system for the duration of this phase. This allows backwards compatibility for users that did not test their workflows in the previous phase.

To access the legacy system:

```bash
ssh login-centos7.rcc.mcw.edu
```

We have legacy partitions (queues) setup to support workflows that do not work on Rocky Linux 8. To run a job on a job with legacy CentOS 7 on a CPU compute node, use `--partition=centos`. To run a job on a job with legacy CentOS 7 on a GPU compute node, use `--partition=centos-gpu`. To run a job on a job with legacy CentOS 7 on a large memory compute node, use `--partition=centos-bigmem`.

!!! warning "Legacy CentOS 7"
    The legacy queues exist to support workflows temporarily. If you did not test your workflows in the previous phase, we suggest you do that and then use the normal queues that now run on Rocky Linux 8. Do not use these queues as a permanent fix for not testing your workflows. If you find your workflow does not work on the upgraded cluster, please contact {{ support_email }} for assistance.

### Final upgrade

***June 5, 2024***

The remaining login and cluster nodes will be upgraded to the new operating system on June 5, 2024. This will conclude the upgrade process.

!!! info
    We may provide further backwards compatibility in the form of a CentOS 7 container. This will not be guaranteed to support your legacy workflow. Contact {{ support_email }} with questions/concerns.

## Next steps for you

When the testing phase begins, please take time to make sure your software functions in the new environment. Please remember to be respectful of all users. Many users will be testing software compatibility on a relatively small set of cluster nodes. Run some jobs to make sure your software works as expected, but please do not shift all of your work.

Report any problems to {{ support_email }}.

## FAQ

??? question "Will this upgrade to Rocky Linux 8 affect reproducibility of my work?"
    Yes, its possible. A change in operating system means updated system libraries, which may affect the performance and output of your code. Concerned users should test their code during the scheduled testing phases as outlined above.

??? question "Will this change the way I submit my jobs?"
    No, we are not changing SLURM as part of this upgrade.

??? question "Will this change how I use OnDemand?"
    No, we are not changing how you interact with OnDemand. However, we will update OnDemand (which we do regularly) and this may update the interface and features.
