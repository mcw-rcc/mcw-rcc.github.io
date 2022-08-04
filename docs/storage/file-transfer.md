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
$ scp local_file user@login-hpc.rcc.mcw.edu:/path/to/remote/target-directory
```

Directory to HPC Cluster:  
```bash
$ scp -r local_directory user@login-hpc.rcc.mcw.edu:/path/to/remote/target-directory
```

File from HPC Cluster:
```bash
$ scp user@login-hpc.rcc.mcw.edu:/path/to/remote_file /path/to/local/target-directory
```

Directory from HPC Cluster:
```bash
$ scp -r user@login-hpc.rcc.mcw.edu:/path/to/remote_directory /path/to/local/target-directory
```

### rsync
Remote sync (rsync) is a fast and secure data transfer tool. Available on Linux and Mac OS X. All commands should be run from the command-line in a terminal app on your computer.

File to HPC Cluster:
```bash
$ rsync -avz local_file user@login-hpc.rcc.mcw.edu:/path/to/target-directory
```

Directory to HPC Cluster:
```
$ rsync -avz local_directory user@login-hpc.rcc.mcw.edu:/path/to/target-directory
```

File from HPC Cluster:
```
$ rsync -avz  user@login-hpc.rcc.mcw.edu:/path/to/remote_file /path/to/local/target-directory
```

Directory from HPC Cluster:
```
$ rsync -avz  user@login-hpc.rcc.mcw.edu:/path/to/remote_directory /path/to/local/target-directory
```

## Desktop Clients
Several software packages are available for data transfer using the secure file transfer protocol (SFTP).

### WinSCP
[WinSCP](https://winscp.net/eng/index.php) is a popular SFTP client for Windows.

### Cyberduck
[Cyberduck](https://cyberduck.io/) is a secure data transfer client available for Windows and Mac OS X users.

Start Cyberduck and select Open Connection. Fill in the information.<br/>
Select SFTP (SSH File Transfer Protocol) from the dropdown menu.<br/>
Server: login-hpc.rcc.mcw.edu<br/>
Port: 22<br/>
Username: MCW username<br/>
Password: MCW password<br/>
Click Connect. 

--8<-- "includes/abbreviations.md"