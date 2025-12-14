# Rclone

Rclone is a command line utility for copying files between cloud servers (i.e. oneDrive, Google Drive, Dropbox, Box, etc) and another server or workstation. All commands should be run from the command-line in a terminal app on your computer.

## Install

To install rclone on a Mac:

```bash
sudo -v ; curl https://rclone.org/install.sh | sudo bash
```

To install rclone on other operating systems, please see the [rclone install documentation](https://rclone.org/install/){:target="_blank"}.

## Configure

To configure a SFTP connection to the cluster:

```bash
rclone config
```

This will guide you through an interactive setup process:

```bash
No remotes found, make a new one\?
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
name> cluster
Type of storage to configure.
Choose a number from below, or type in your own value
Storage> sftp
SSH host to connect to
Choose a number from below, or type in your own value
 1 / Connect to example.com
   \ "example.com"
host> login-hpc.rcc.mcw.edu
SSH username
Enter a string value. Press Enter for the default ("$USER").
user> NetID # substitute your MCW username
SSH port number
Enter a signed integer. Press Enter for the default (22).
port>
SSH password, leave blank to use ssh-agent.
y) Yes type in my own password
g) Generate random password
n) No leave this optional password blank
y/g/n> n
Path to unencrypted PEM-encoded private key file, leave blank to use ssh-agent.
key_file>
Remote config
Configuration complete.
```

We just configured a connection to the cluster, but rclone can be used with many other endpoints, including cloud services. For a list of possible endpoints and detailed information on how to configure each, please see the [rclone documentation](https://rclone.org/docs/){:target="_blank"}.

## File transfer

Copying a file or directory to the HPC cluster:

```bash
rclone copy /path/to/source cluster:/path/to/target
```

Copying a file or directory from the HPC cluster to your computer:

```bash
rclone copy cluster:/path/to/source /path/to/target
```
