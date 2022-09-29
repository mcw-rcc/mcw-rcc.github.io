# Storage Overview

RCC offers multiple types of storage to all users. Each storage type has a unique path on the cluster, and a unique use-case. Some storage is meant only for use with the cluster, while other storage like [Research Group Storage](research-group-storage.md) can be used as generalized Windows storage.

!!! tip "You can easily find your available storage directories and current utilization on the cluster  with the `mydisks` command."

    ```txt
    $ mydisks
    =====My Lab=====
    Size  Used Avail Use% File
    47G   29G   19G  61% /home/user
    932G  158G  774G  17% /group/pi
    4.6T     0  4.6T   0% /scratch/u/user
    4.6T     0  4.6T   0% /scratch/g/pi
    ```

## Hardware

The storage systems provide large scale data storage at high performance, including an NVMe all-flash scratch space for the cluster and a scale out NAS storage for research group data. Data storage is controlled by quotas that provide a certain amount of storage for free. Additionally, the scale-out NAS storage features snapshots to avoid data loss from accidental deletion. However, none of the storage is backed up, i.e. there is no disaster recovery.

|                       | HPC Scratch | [Research Group Storage](research-group-storage.md) |
| --------------------- | ----------- | ----------------------------- |
| Vendor | Qumulo | Qumulo |
| Type | Tier0 NVMe All Flash | Tier2 Scale-out NAS |
| Year Purchased | 2020 | 2020 |
| Total Usable Space | 215TB | 1.77PB |
| Connectivity | 100Gbps | 10Gbps |
| Quotas | 5TB limit | 1TB free (billable after 1TB) |
| Snapshots | None | 7 days |
| Tape Backup | None | None |

## Cluster Storage

Each cluster mounts the file systems in the following locations.

### /home

Every user has a home directory located at `/home/{NetID}`. The home directory is used for storing user-installed software, user-written scripts, etc. Each home directory is only accessible by its owner. This directory is not suitable for sharing files. The quota limit is 50GB.

### /group i.e. "Research Group Storage"

[Research Group Storage](../storage/research-group-storage.md), located at `/group`, is meant for active research files that need to be shared by members of a research group. Each participating PI will have a free 1TB storage directory. Additional storage capacity may be purchased with a [Research Group Storage subscription](../storage/research-group-storage.md#paid-additional-storage).

!!! tip "Do you need to collaborate outside your lab group?"
    Your lab group storage can be used to collaborate with non-lab members. This is done with by-request project directories. For example, if lab `pi` would like to collaborate with some data for a project called `zephyr`, then the lab PI would contact RCC to create a project directory.

    ```bash
    $ ls -l /group/pi
    total 8
    drwxrws---. 3 root sg-pi-zephyr 512 Mar 26 13:52 zephyr
    drwxrws---. 4 root sg-pi       1024 May 19 14:04 work
    ```

    In addition to the usual `work` directory, there is now a `zephyr` project directory, which is controlled by the `sg-pi-zephyr` security group. Project data would go in that directory, and any number of collaborators (MCW researchers) can be added to the security group.

### /scratch

Scratch space is intended for data that is read/written during jobs running on the cluster. HPC includes a new scratch space built on NVMe storage. It is located on all compute nodes at `/scratch`. Every user has a directory at `/scratch/u/{NetID}` with quota limit 5TB. Every group has a directory at `/scratch/g/{PI_NetID}` with quota limit 5TB. Scratch space may be increased by request with justification.

!!! tip "Do you need more scratch storage?"
    Scratch storage may be increased beyond the 5TB limit. To increase scratch storage, send an email to {{ support_email }} with the amount and justification.

### /tmp

A 480GB local scratch disk is available on each compute node at `/tmp`.
!!! tip "`/tmp` is fast, but limited space."
    Local scratch disk may be the fastest computing option to store your job runtime files. This may be true with jobs that are heavily I/O dependent (i.e. lots of files are read/written). However, there is a time penalty to transfer your files from your global scratch directory to local scratch, and the space is limited (480GB). Please contact {{ support_email }} with questions.

### Using Storage with Jobs

HPC job workflow:

1. User copies job input/supporting files from an RGS directory within `/group/{PI_NetID}/...` to their scratch directory `/scratch/u/{NetID}` or `/scratch/g/{PI_NetID}`
2. User submits job that computes with the staged job input/supporting files
3. Job finishes and user copies results from `/scratch/u/{NetID}` or `/scratch/g/{PI_NetID}`, back to `/group/{PI_NetID}/...`
4. User continues with further computations with the job input/supporting files
5. User finishes computations and deletes unneeded job input/supporting files from `/scratch/u/{NetID}` or `/scratch/g/{PI_NetID}`

## Non-cluster Storage

[Research Group Storage](../storage/research-group-storage.md) is also available via an SMB mount on your Windows or Mac desktop. This access is convenient, but cannot be used to access files that you have saved via your cluster access. In other words, you can save and utilize data via your cluster access (Linux), or via this access (Windows), but the data cannot be accessed from both. Contact {{ support_email }} to request Windows access to your storage space.

--8<-- "includes/abbreviations.md"
