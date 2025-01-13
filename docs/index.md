# Research Computing Documentation

## Introduction

Research Computing provides services and support for computational research at MCW. Our primary focus is High Performance Computing (HPC) and research data storage. We also work with MCW investigators to facilitate adoption and use of these advanced computational resources.

## Services

### HPC Cluster

The {{ hpc_name }} cluster is the institution's primary computational resource, and has been available to MCW researchers since March 2021. The cluster contains over **3,800** CPU cores in a variety of compute node architectures, including large memory and GPUs. All compute nodes are connected by a 100 Gbps RoCEv2 network. The cluster also includes a **467 TB** NVMe scratch storage filesystem. Please see the [Quick Start guide](cluster/quickstart.md) for more detail.

### ResHPC

**Res**tricted **HPC** (ResHPC) is a secure way to access and utilize the HPC cluster. It is specifically designed for restricted datasets that have a defined Data Use Agreement (DUA), including dbGaP projects. The ResHPC service is built on the existing HPC cluster, but incorporates a separate, secure login method, and project specific accounts and directories. With ResHPC, you can work with familiar tools while also satisfying complex data provider security requirements. Please see the [ResHPC overview](secure-computing/reshpc.md) to get started.

### Data Storage

In addition to the cluster's scratch storage, RCC also provides general purpose research storage with a replicated **2.6 PB** filesystem. This persistent storage is mounted on the cluster via NFS, or provided directly to user's via NFS and SMB. Please see the [Storage Overview](storage/rcc-storage.md) for more detail.

### Software

Cluster software installation and tuning services are available to all users. We will help install most supported software packages on the clusters, and when able, will advise on best use for our clusters. This might include advice on data staging, parallelization, memory utilization, etc. Please note that team members are not domain knowledge experts in the sciences, and will not advise you on the accuracy or applicability of any software package for your research. You can [request software installation](software/module-request.md) today.

### Consulting

We are glad to help with your research data and computing needs. Consulting topics might include how best to use the cluster for a workflow, software install, research data management, help with grants, security, etc. Project time, software development, paid support, and other dedicated resources are not available.

## Support and Training

### Email

Contact Research Computing support at {{ support_email }}.

### Virtual Office Hours

Virtual office hours are available via Zoom. Please email {{ support_email }} to schedule an appointment.

### Workshops

RCC is planning workshops on a variety of topics. These are meant to include multiple levels of expertise and cover such topics as HPC, scripting, containers, etc. If you have a suggestion for a new workshop, please contact {{ support_email }}.

| Title | Level | Slides |
| ----- | ----- | ------ |
| HPC Cluster Onboarding | Introductory | [Download](_static/files/HPC_Cluster_Onboarding_2022.pdf){:target="_blank"} |

## Acknowledgement

Please include the following acknowledgement in any publication resulting from work on Research Computing resources:

!!! example "Acknowledgement"

    This research was completed in part with computational resources and 
    technical support provided by the Research Computing Center at the 
    Medical College of Wisconsin.

Check out the [list of publications](pubs.md) to see how Research Computing is enabling science at MCW.
