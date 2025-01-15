# File Transfer

Several methods are available for transferring data to/from the HPC Cluster. These include [Open OnDemand](../cluster/access/ondemand.md), command-line, and desktop client software. RCC recommends Open OnDemand for all users, especially for remote work.

## Open OnDemand

Open OnDemand is a web portal for using HPC and includes a file management app. For more information, see [Open OnDemand Files App](../cluster/access/ondemand.md#file-management).

## Command-line

Several command-line options are available for secure data transfer.

### scp

Secure copy (scp) is a tool for secure data transfer between UNIX-like systems using your MCW username and password. Available on Linux and Mac OS X. All commands should be run from the command-line in a terminal app on your computer.

Copy a file to the HPC Cluster:

```bash
scp local_file user@login-hpc.rcc.mcw.edu:/path/to/remote/target-directory
```

Copy a directory to the HPC Cluster:**

```bash
scp -r local_directory user@login-hpc.rcc.mcw.edu:/path/to/remote/target-directory
```

Copy a file from the HPC Cluster:

```bash
scp user@login-hpc.rcc.mcw.edu:/path/to/remote_file /path/to/local/target-directory
```

Copy a directory from the HPC Cluster:

```bash
scp -r user@login-hpc.rcc.mcw.edu:/path/to/remote_directory /path/to/local/target-directory
```

### rsync

Remote sync (rsync) is a fast and secure data transfer tool. Available on Linux and Mac OS X. All commands should be run from the command-line in a terminal app on your computer.

Copy a file to the HPC Cluster:

```bash
rsync -avz local_file user@login-hpc.rcc.mcw.edu:/path/to/target-directory
```

Copy a directory to the HPC Cluster:

```bash
rsync -avz local_directory user@login-hpc.rcc.mcw.edu:/path/to/target-directory
```

Copy a file from the HPC Cluster:

```bash
rsync -avz  user@login-hpc.rcc.mcw.edu:/path/to/remote_file /path/to/local/target-directory
```

Copy a directory from the HPC Cluster:

```bash
rsync -avz  user@login-hpc.rcc.mcw.edu:/path/to/remote_directory /path/to/local/target-directory
```

### rclone

RClone is a command line utility for copying files between cloud servers (i.e. oneDrive, Google Drive, Dropbox, Box, etc) and another server or workstation. All commands should be run from the command-line in a terminal app on your computer.

#### Running rclone in your local computer

Install RClone:

```bash
sudo -v ; curl https://rclone.org/install.sh | sudo bash
```

Configure the connection to the RCC:

```bash
rclone config
```

Then, follow the instructions in the command line, leaving any default values as they are. Select option 48 (ssh) for the type of storage. Write login-hpc.rcc.mcw.edu for the host. SSH username is the same that you use to login to the cluster. Finally, when asked about the SSH password, select y (type in my own password). [Here](https://rclone.org/sftp/){:target="_blank"} you will find more detailed instructions. This configuration only needs to be done once.

Configure the connection to the Cloud:

This configuration depends on the cloud service that you wish to connect. For a list of cloud servers and detailed information on how to configure each of them please follow the instructions in this page: [RClone configuration](https://rclone.org/docs/){:target="_blank"}. This configuration only needs to be done once.

Copying a file or directory from the cloud to the HPC Cluster:

```bash
rclone copy cloud:/path/to/file_or_folder user@login-hpc.rcc.mcw.edu:/path/to/target-directory
```

Replace ```cloud``` by the name you gave to your cloud connection during the configuration above.

Copying a file or directory from the HPC Cluster to the cloud:

```bash
rclone copy user@login-hpc.rcc.mcw.edu:/path/to/target-directory cloud:/path/to/file_or_folder
```

Replace ```cloud``` by the name you gave to your cloud connection during the configuration above.

#### Running rclone in the RCC

Configure the connection to the cloud:

In order to do this configuration, you will need a web browser. First, connect to [onDemand](https://ondemand.rcc.mcw.edu/){:target="_blank"} and login with your credentials. Go to Interactive Apps on top and select Remote Desktop. Then, launch a new interactive session. Your remote desktop session might take a few minutes to be ready, depending on the number of cores, the time requested and how overloaded is the system at the time.

```bash
module load rclone
```

The instructions on how to configure the connection to the cloud, as mentioned above, depend on the cloud service that you are connecting. For a list of cloud servers and detailed information on how to configure each of them please follow the instructions in this page: [RClone configuration](https://rclone.org/docs/){:target="_blank"}. When the browser window opens, log into your account and click the Grant Access link that appears. This configuration only needs to be done once.

You can now use rclone in the Remote Desktop or in your terminal when connected to the RCC.

Copying a file or directory from the cloud to the HPC Cluster:

```bash
rclone copy cloud:/path/to/file_or_folder /path/to/target-directory
```

Replace ```cloud``` by the name you gave to your cloud connection during the configuration above.

Copying a file or directory from the HPC Cluster to the cloud:

```bash
rclone copy /path/to/target-directory cloud:/path/to/file_or_folder
```

Replace ```cloud``` by the name you gave to your cloud connection during the configuration above.

For a full list of commands visit [The RClone Commands page](https://rclone.org/commands/){:target="_blank"}

## Desktop Clients

Several software packages are available for data transfer using the secure file transfer protocol (SFTP).

- [CoreFTP](https://servicedesk.mcw.edu/){:target="_blank"} is recommended and licensed by MCW-IS. Use the link to login and search for CoreFTP in the Software section.
- [MobaXterm](../cluster/access/mobaxterm.md#file-transfer) has a built-in SFTP client.
- [WinSCP](https://winscp.net/eng/index.php){:target="_blank"} is a popular SFTP client for Windows.
- [Cyberduck](https://cyberduck.io/){:target="_blank"} is a secure data transfer client available for Windows and Mac OS X users.

--8<-- "includes/abbreviations.md"
