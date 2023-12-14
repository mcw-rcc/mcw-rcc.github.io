# HPC Cluster Operating System Upgrade

The HPC cluster operating system, CentOS 7, will be end-of-life in June 2024 and we are beginning a project to upgrade. This will allow us to maintain ongoing security patching and compatibility with new software through the cluster lifetime. To that end, we have chosen to upgrade the HPC cluster to Rocky Linux 8.

We are currently working on a smooth upgrade path that will minimize downtime and disruption to workflow. This includes testing most centrally installed software packages for compatibility. When necessary, we will rebuild those software packages while maintaining version if possible. We do anticipate that certain workflows will be disrupted, especially for users that have installed their own software.

## Potential issues

- System Python2 will no longer be installed. Python2 has been end-of-life and unsupported since 2020. If you have workflows built on Python2, upgrade the code to Python3. If you must use Python2, you may use the centrally installed modulefile `python/2.7.18`.
- System library versions will change. System libraries such as GCC will have newer versions in Rocky Linux 8. If you have compiled your workflow with system default libraries, you will need to rebuild.

## Timeline

The scheduled dates below are subject to change. Please check regularly for updates.

### User testing phase

***December 13, 2023 - April 3, 2024***

A login node and a small number of cluster nodes will be upgraded to enable testing starting November 6, 2023.

To login to this testing system:

```bash
ssh login-rocky.rcc.mcw.edu
```

We have new partitions (queues) to test your workflows. To run a job on a CPU compute node, use `--partition=rocky`. To run a job on a GPU compute node, use `--partition=rocky-gpu`. At this time, large memory nodes are not included in this test. Please use the CPU nodes to test those workflows. Contact {{ support_email }} for assistance.

### Upgrade and extended testing

***April 3, 2024 - June 5, 2024***

On April 3, 2024, we will upgrade a majority of login and cluster nodes. We will continue to maintain a login node and small number of cluster nodes on the legacy CentOS 7 operating system. This will ensure backwards compatibility for those users that have not fixed software issues prior to upgrade.

### Final upgrade

***June 5, 2024***

The remaining login and cluster nodes will be upgraded to the new operating system on June 5, 2024. This will conlude the upgrade process.

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
