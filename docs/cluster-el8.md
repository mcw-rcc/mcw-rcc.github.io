# HPC Cluster Operating System Upgrade

The HPC cluster operating system, CentOS 7, will be end-of-life in June 2024 and we are beginning a project to upgrade. This will allow us to maintain ongoing security patching and compatibility with new software through the cluster lifetime. To that end, we have chosen to upgrade the HPC cluster to Rocky Linux 8.

We are currently working on a smooth upgrade path that will minimize downtime and disruption to workflow. This includes testing most centrally installed software packages for compatibility. When necessary, we will rebuild those software packages while maintaining version if possible. We do anticipate that certain workflows will be disrupted, especially for users that have installed their own software.

## Potential issues

- Python2 will no longer be installed. Python2 has been end-of-life and unsupported since 2020. If you have workflows built on Python2, you will have to upgrade to Python3 or reinstall yourself.
- System library versions will change. System libraries such as GCC will have newer versions in Rocky Linux 8. If you have compiled your workflow with system default libraries, you will need to rebuild.

## Timeline

Please check regularly for updates.

### User testing phase

***November 6, 2023 - March 5, 2024***

A login node and a small number of cluster nodes will be upgraded to enable testing starting November 6, 2023.

### Upgrade and extended testing

***March 6, 2024 - April 30, 2024***

Most login nodes and cluster nodes will be upgraded March 6, 2024. A login node and small number of cluster nodes will remain on the legacy CentOS 7 operating system until April 30, 2024. This will ensure backwards compatibility for those users that have not fixed software issues prior to upgrade.

### Final upgrade

***May 1, 2024***

Remaining nodes will be upgraded to the new operating system on May 1, 2024. We may provide further backwards compatibility in the form of a CentOS 7 container.

## Next steps for you

When the testing phase begins, please take time to make sure your software functions in the new environment. Please remember to be respectful of all users. Many users will be testing software compatibility on a relatively small set of cluster nodes. Run some jobs to make sure your software functions, but please do not run full-scale workloads that would monopolize the resources.

Report any problems to {{ support_email }}.
