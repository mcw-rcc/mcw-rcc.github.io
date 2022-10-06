# Storage Overview

Research Computing provides storage in two tiers meant to support both large scale and high performance. Every MCW lab is eligible for a limited amount of free storage. For many labs, this amount of free storage is sufficient for their research. For labs with large data needs, additional storage is available for fee.

All storage is connected via high speed link to the cluster and available via Linux command-line, SFTP, or Open OnDemand. In addition, some storage is available directly to users via Windows or NFS shares.

??? tip "You can easily find your available storage paths and current utilization on the cluster  with the `mydisks` command."

    ```bash
    $ mydisks
    =====My Lab=====
    Size  Used Avail Use% File
    47G   29G   19G  61% /home/user
    932G  158G  774G  17% /group/pi
    4.6T     0  4.6T   0% /scratch/u/user
    4.6T     0  4.6T   0% /scratch/g/pi
    ```

## Storage Paths

### Home

**Quota**: 50 GiB  
**Snapshot**: daily @ 12PM, 7 day lifetime  
**Replication**: continuous with snapshot

Every user has a home directory located at `/home/netid`. The home directory is used for storing user-installed software, user-written scripts, etc. Each home directory is only accessible by its owner. This directory is not suitable for sharing files. The quota limit is 50GiB.

### Group

**Quota**: 1 TiB, paid additional  
**Snapshot**: daily @ 2PM, 7 day lifetime  
**Replication**: continuous with snapshot

Every lab is eligible for a group directory at `/group/pi_netid` with a free 1TiB limit. This space is meant for research data in active projects. This space is large scale, but low performance. It is not meant for high I/O, and so is not mounted to compute nodes.

Additional storage capacity may be purchased through our Research Group Storage service in 1TiB increments for $60/TiB/year. Please see [Paid Additional Storage](../storage/paid-storage.md) for details.

### Scratch

**Quota**: 5 TiB, expandable to 10 TiB  
**Purge**: files may be deleted after 180 days

Scratch space is intended for data that is read/written during jobs running on the cluster. It is located on all compute nodes at `/scratch`. Every user has a directory at `/scratch/u/netid` with quota limit 5TiB. Every group has a directory at `/scratch/g/pi_netid` with quota limit 5TiB. Scratch space may be increased by request with justification.

!!! tip "Do you need more scratch storage?"
    Scratch storage may be increased beyond the 5TiB limit. To increase scratch storage, send an email to {{ support_email }} with the amount and justification.

### Local Scratch

**Quota**: No quota, limited to size of disk (440GiB)  
**Purge**: files deleted after every job

Local scratch disk may be the fastest computing option to store your job runtime files, especially for jobs that are heavily I/O dependent (i.e. lots of files are read/written). However, it does take some time to transfer your files from your global scratch directory to local scratch, and the space is limited (440GiB).

## Permissions

Every lab storage path will have an associated security group consisting of the PI and additional users that the PI adds. We require two points of contact that are authorized to request permissions changes. The PI will serve as one point of contact and will provide an alternate. Any group permission changes must come from the PI or alternate. ***Requests must be made through the MCW ticketing system.***

## Restrictions

The following types of data are strictly prohibited on Research Computing systems:

- Any data that would violate the MCW Code of Conduct, MCW Corporate Polices, or any applicable data-use agreement (i.e. IRB, federal grant regulations, etc.)

- Any data that is subject to HIPAA

## Data Protection

All Research Computing storage systems are highly resilient, allowing for multiple disk and server failures without losing data. We also maintain support contracts for all storage with provisions for prompt hardware replacement. In addition, some file systems have additional protection/features.

!!! info "Disclaimer"
    Research Computing is not responsible for any loss of data. We strongly encourage all users to follow best practice data management strategies.

--8<-- "includes/abbreviations.md"
