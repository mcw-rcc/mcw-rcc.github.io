# Storage Overview

Research Computing provides storage with a dedicated purpose to hold and support analysis of raw research data. Every MCW lab is eligible for a limited amount of free storage. For many labs, this amount of free storage is sufficient for their research. For labs with large data needs, additional storage is available for fee.

All storage is connected via high speed link to the cluster and available to you via Linux command-line, SFTP, or Open OnDemand.

Each user has the same set of default storage paths:

| Type                  | Path                | Quota                         | Protection            | Description                       |
| --------------------- | ------------------- | ----------------------------- | --------------------- | --------------------------------- |
| [`Home`](#home)       | /home/netid         | 100 GB                        | snapshot, replication | account configuration and scripts |
| [`Group`](#group)     | /group/pi_netid     | 5 TB, expandable with payment | snapshot, replication | shared raw research data          |
| [`Scratch`](#scratch) | /scratch/g/pi_netid | 25 TB                         | none                  | temporary job files               |

??? tip "You can easily find your available storage paths and current utilization on the cluster  with the `mydisks` command."

    ```bash
    $ mydisks
    =====My Lab=====
    Size  Used Avail Use% File
    47G   29G   19G  61% /home/user
    932G  158G  774G  17% /group/pi
    4.6T     0  4.6T   0% /scratch/g/pi
    ```

## Storage Paths

### Home

The home directory is your starting place every time you login to the cluster. It's location is `/home/netid`, where `netid` is your MCW username. The purpose of a home directory is storing user-installed software, user-written scripts, configuration files, etc. Each home directory is only accessible by its owner and is not suitable for data sharing. Home is also not appropriate for large scale research data or temporary job files.

The quota limit is 100 GB and data protection includes replication and snapshots. For more info on snapshots, and how you might recover a file, please see [file recovery](file-recovery.md).

### Group

Group storage is a shared space for labs to store research data in active projects. Each lab receives 5 TB for free and can expand via [additional paid storage](../storage/paid-storage.md). This space is large scale, but low performance. It is not meant for high I/O, and so is not mounted to compute nodes. Data protection includes replication and snapshots. For more info on snapshots, and how you might recover a file, please see [file recovery](file-recovery.md).

This space is organized by lab group. Each folder in `/group` represents a lab, and is named using the PI's NetID (username). For example, a PI with username "jsmith" would have a group directory located at `/group/jsmith`. Directories within that lab space are organized by purpose and controlled by unique security groups. For example, there is a default `/group/pi_netid/work` directory, which is shared space restricted to lab users. Other shared directories can be created by request for projects that require unique permissions. Additionally, you may have data directories related to your use of a MCW core. These directories will be named for the core and located at `/group/pi_netid/cores`. For example, a Mellowes Center project could be delivered to your group storage and located at `/group/pi_netid/cores/mellowes/example_project1`.

### Scratch

Scratch storage is intended for temporary job files. Every group has a directory at `/scratch/g/pi_netid` with quota limit 25 TB. Files on scratch storage are subject to retention limits, which is discussed below. In general, you should avoid storing files on scratch unless you are running a job. Please remember that scratch storage is limited and shared among all groups.

#### Retention

Any file that is older than 60 days (based on creation, modification, or access time) may be deleted by RCC admins without notice. RCC sends periodic reminders to clean up scratch storage directories. These reminders will include your percentage of files older than 60 days. A list of these old files can be provided upon request. If the scratch file system becomes full, RCC admins may begin deleting files without notice, starting with files older than 60 days, followed by any other files as needed to free up space.

!!! warning "Scratch storage is for jobs, not projects."

    Do not use scratch storage for long-term project data. If you are not running a job, you should not have any data in scratch. Failure to adhere to this policy may result in loss of your data located on scratch.

### Local Scratch

Every compute node has a local scratch space to be used for runtime files. Each job will have a unique folder that appears as `/tmp`, which is only accessible to processes within the SLURM job. Please note, this space is cleaned (data deleted) after each of your jobs.

Local scratch may be the fastest option to store your job runtime files, especially for jobs that are heavily I/O dependent (i.e. lots of files are read/written). However, the speed of the disk should be weighed against the additional time to transfer files from your global scratch directory.

All compute nodes have 440 GB of local scratch storage, except as noted below.

!!! tip "Some GPU nodes have more local scratch."

    Compute nodes gn07 and gn08 have 17 TB of local storage and gn09 has 7 TB. These nodes are intended for large scale AI jobs that require local fast storage.

## Permissions

Every lab storage path will have an associated security group consisting of the PI and additional users that the PI adds. We require two points of contact that are authorized to request permissions changes. The PI will serve as one point of contact and will provide an alternate. Any group permission changes must come from the PI or alternate. ***Requests must be made through the MCW ticketing system.***

## Restrictions

The following types of data are strictly prohibited on Research Computing systems:

- Any data that would violate the MCW Code of Conduct, MCW Corporate Polices, or any applicable data-use agreement (i.e. IRB, federal grant regulations, etc.)

- Any data that is subject to HIPAA

## Data Protection

All Research Computing storage systems are highly resilient, allowing for multiple disk and server failures without losing data. We also maintain support contracts for all storage with provisions for prompt hardware replacement. In addition, some file systems have additional protection.

Home directories and research group storage, i.e., `/group/pi_netid`, are protected by snapshots and replication. Replication occurs via two identical storage systems located in our two data centers. Replication occurs every minute (continuously), with changes in the primary file system sent to the target file system. These changes might include new files, edits, deletions, etc. Replication is a disaster recovery measure, protecting against the loss of a full file system. Since all data changes are replicated, it does not protect against accidental or unintended edits, overwrites, deletions, etc.

To protect against these unintended changes, we use snapshots. The granularity of snapshots determines which of these changes are captured. It is not guaranteed that all changes will be captured by snapshots, and therefore recoverable. For instance, we keep 14 daily and 6 weekly snapshots. If an unintended change is caught by the snapshot, i.e. you delete a file that has been on the file system for more than 24 hrs, you may be able to recover it. However, snapshots may miss changes that occur within 24 hrs. For instance, if you create and delete a file before the daily snapshot can capture a version of the file, then it would not be recoverable.

!!! info "Disclaimer"
    Research Computing is not responsible for any loss of data. We strongly encourage all users to follow best practice data management strategies.

--8<-- "includes/abbreviations.md"
