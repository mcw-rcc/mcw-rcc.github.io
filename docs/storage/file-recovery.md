# File Recovery

## Definition
A snapshot is a read-only copy of a file at a point in time. A file system uses snapshots to capture incremental changes to files and saves them on disk. Since only the changes are saved, it does not require many full copies of the dataset, which saves space and time.

## Data Protection
A snapshot can protect against accidental deletions or changes to files. However, this only works in the case that a file is captured. If a file is created and changed/deleted before a snapshot occurs, then the file is not protected and the previous version will not help to recover.

!!! danger "NOT A BACKUP"
    A snapshot is not a backup. Since a snapshot is saved on the file system, it cannot be used in a disaster recovery event where the file system is lost.

## RCC Policy
RCC uses snapshots on the `/group` and `/home` file paths. The RCC file system captures a daily snapshot with `/home` at 12PM and `/group` at 2PM. Each snapshot is saved for 7 days before it is deleted.

## Recover file from snapshot
!!! warning "This procedure will not work in every case!"
    Sometimes a file is created and deleted before a snapshot can be completed. For example, home directories are snapshotted once daily. If you cannot locate your file with the following procedures, contact {{ support_email }}.

### Linux (All Clusters)
To recover files in your directory, use the `.snapshot` directory. 
!!! tip "Commands must reference `.snapshot` directly."
    The `.snapshot/` directory is hidden from standard Linux tools. Consider the following example:
    ```bash
    $ ls -la /home/user # will not show .snapshot/
    $ ls -la /home/user/.snapshot/ # displays the contents of .snapshot/
    ```

Then select a snapshot directory. Snapshot directories are numbered with the largest number being the latest snapshot. However, if you would like to see the time points, use command `ls -l`. Once you select a snapshot directory, you can navigate your files. Then copy back the file you need.

```bash
$ ls -l .snapshot/
total 112
drwx------. 92 user sg-pi 73728 May 20 12:00 543_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 21 12:00 545_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 22 12:00 547_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 23 12:00 549_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 24 12:00 551_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 25 12:00 553_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 26 12:00 555_Home Directory_homefs
$ ls .snapshot/555_Home Directory_homefs/
file1 file2 file3

$ cp .snapshot/555_Home Directory_homefs/file1 /home/user
$ ls /home/user/
file1
```

### Windows
To recover a file in your Windows share:

1. Open the folder that previously contained your lost file.
2. Right-click in the window and select **Properties**.
3. Select the **Previous Versions** tab.
4. Select the version you would like to explore.
5. In the new window, locate the file you want to recover.
6. Copy the file or folder to the previous location.

!!! tip "**Previous Versions** are timestamped."
    Look for the timestamped version that may contain your file. For instance, if you deleted a file Tuesday, then try a timestamped version from Monday. This may help you find the file or version of the file that you need.}}

--8<-- "includes/abbreviations.md"