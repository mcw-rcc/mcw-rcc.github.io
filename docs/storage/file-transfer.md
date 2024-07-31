# File Transfer

Several methods are available for transferring data to/from the HPC Cluster. These include [Open OnDemand](../user-guide/access/ondemand.md), command-line, and desktop client software. RCC recommends Open OnDemand for all users, especially for remote work.

## Open OnDemand

Open OnDemand is a web portal for using HPC and includes a file management app. For more information, see [Open OnDemand Files App](../user-guide/access/ondemand.md#file-management).

## Command-line

Several command-line options are available for secure data transfer.

### scp

Secure copy (scp) is a tool for secure data transfer between UNIX-like systems using your MCW username and password. Available on Linux and Mac OS X. All commands should be run from the command-line in a terminal app on your computer.

File to HPC Cluster:

```bash
scp local_file user@login-hpc.rcc.mcw.edu:/path/to/remote/target-directory
```

Directory to HPC Cluster:

```bash
scp -r local_directory user@login-hpc.rcc.mcw.edu:/path/to/remote/target-directory
```

File from HPC Cluster:

```bash
scp user@login-hpc.rcc.mcw.edu:/path/to/remote_file /path/to/local/target-directory
```

Directory from HPC Cluster:

```bash
scp -r user@login-hpc.rcc.mcw.edu:/path/to/remote_directory /path/to/local/target-directory
```

### rsync

Remote sync (rsync) is a fast and secure data transfer tool. Available on Linux and Mac OS X. All commands should be run from the command-line in a terminal app on your computer.

File to HPC Cluster:

```bash
rsync -avz local_file user@login-hpc.rcc.mcw.edu:/path/to/target-directory
```

Directory to HPC Cluster:

```bash
rsync -avz local_directory user@login-hpc.rcc.mcw.edu:/path/to/target-directory
```

File from HPC Cluster:

```bash
rsync -avz  user@login-hpc.rcc.mcw.edu:/path/to/remote_file /path/to/local/target-directory
```

Directory from HPC Cluster:

```bash
rsync -avz  user@login-hpc.rcc.mcw.edu:/path/to/remote_directory /path/to/local/target-directory
```

## Desktop Clients

Several software packages are available for data transfer using the secure file transfer protocol (SFTP).

- [CoreFTP](https://servicedesk.mcw.edu/){:target="_blank"} is recommended and licensed by MCW-IS. Use the link to login and search for CoreFTP in the Software section.
- [MobaXterm](../user-guide/access/mobaxterm.md#file-transfer) has a built-in SFTP client.
- [WinSCP](https://winscp.net/eng/index.php){:target="_blank"} is a popular SFTP client for Windows.
- [Cyberduck](https://cyberduck.io/){:target="_blank"} is a secure data transfer client available for Windows and Mac OS X users.

--8<-- "includes/abbreviations.md"
