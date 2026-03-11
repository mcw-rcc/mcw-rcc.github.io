# Transfer to Cluster

Several methods are available for transferring data to/from the HPC Cluster. These include [Open OnDemand](../cluster/access/ondemand.md), command-line, and desktop client software. RCC recommends Open OnDemand for all users, especially for remote work.

## Open OnDemand

Open OnDemand is a web portal for using HPC and includes a file management app. For more information, see [Open OnDemand Files App](../cluster/access/ondemand.md#file-management).

## Command-line utilities

Several command-line options are available for secure data transfer.

### scp

Secure copy (scp) is a tool for secure data transfer between UNIX-like systems using your MCW username and password. Available on Linux and Mac OS X. All commands should be run from the command-line in a terminal app on your computer.

Copy a file to the HPC Cluster:

```bash
scp local_file user@login-hpc.rcc.mcw.edu:/path/to/remote/target-directory
```

Copy a directory to the HPC Cluster:

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

## FTP clients

Several software packages are available for data transfer using the secure file transfer protocol (SFTP).

| Tool | Platforms | Pros | Cons | Heavy transfers |
| --- | --- | --- | --- | --- |
| CoreFTP | [Windows](https://servicedesk.mcw.edu/){:target="_blank"} | Lightweight, simple GUI | Windows only, no SSH Terminal, no windowed apps | Good for heavy transfers with resume, bandwidth control and scheduling |
| MobaXterm | [Windows](../cluster/access/mobaxterm.md) | SSH Terminal, run windowed apps from the cluster | Windows only, heavier app | Not optimized for big multi-threaded queues |
| WinSCP | [Windows](https://winscp.net/eng/download.php){:target="_blank"} | Scripting to automate file transfers | Windows only, no SSH Terminal, no Windowed apps | Good for long SFTP jobs, reliable queues and resume |
| Cyberduck | [Windows](https://cyberduck.io/download/){:target="_blank"}, [Mac](https://cyberduck.io/download/){:target="_blank"} | User-friendly GUI, supports multiple storage types, external editor integration | No synchronization, memory heavy, no SSH Terminal | Slow for large transfers |

--8<-- "includes/abbreviations.md"
