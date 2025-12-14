# Data Recovery

This guide explains how you might recover a file using a data protection feature called a snapshot. Snapshots are available on your home and group directories. For reference, a snapshot is an on-disk reference point of file changes from a time period. It is very similar to file versions in Microsoft products. This feature allows you to look back at previous versions of your files, and recover when needed. Below you'll find info on how to recover a file, when possible.

!!! warning "This procedure will not work in every case!"
    Sometimes a file is created and deleted before a snapshot can be completed. For example, home directories are snapshotted once daily. If you cannot locate your file with the following procedures, contact {{ support_email }}.

## Linux (All Clusters)

To recover files in a directory, access the `.snapshot` directory. Snapshot directories are numbered with the largest number being the latest snapshot. However, if you would like to see the time points, use command `ls -l`.

```bash
ls -l .snapshot/
# snapshots listed with timestamp
drwx------. 92 user sg-pi 73728 May 20 12:00 543_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 21 12:00 545_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 22 12:00 547_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 23 12:00 549_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 24 12:00 551_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 25 12:00 553_Home Directory_homefs
drwx------. 92 user sg-pi 73728 May 26 12:00 555_Home Directory_homefs
```

Once you select a snapshot directory, you can navigate to your files. Then copy back the file you need.

```txt
cp /home/user/.snapshot/555_Home Directory_homefs/file1 /home/user
```

!!! tip "Commands must reference `.snapshot` directly."
    The `.snapshot/` directory is hidden from standard Linux tools. Consider the following example:
    <!-- markdownlint-disable MD046 -->
    ```bash
    ls -la /home/user # will not show .snapshot/
    ls -la /home/user/.snapshot/ # displays the contents of .snapshot/
    ```
    <!-- markdownlint-enable MD046 -->

## Windows

To recover a file in your Windows share:

1. Open the folder that previously contained your lost file.
2. Right-click in the window and select **Properties**.
3. Select the **Previous Versions** tab.
4. Select the version you would like to explore.
5. In the new window, locate the file you want to recover.
6. Copy the file or folder to the previous location.

## Help

Remember, snapshots are timestamped. Look for the timestamped version that may contain your file. For instance, if you deleted a file Tuesday, then try a timestamped version from Monday. Contact {{ support_email }} with questions.

--8<-- "includes/abbreviations.md"
