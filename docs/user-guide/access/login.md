# Logging in

Login is available via web browser app and traditional SSH connection. All logins require your MCW username and password. [Remote access](remote-access.md) via these methods is also available with the addition of MCW Citrix or VPN.

## Web Browser

Open OnDemand is a web browser-based interface to RCC computing resources. You can manage files, submit and monitor jobs, and run pre-configured interactive apps such as Jupyter and RStudio. All of this is possible without logging in via a traditional SSH client.

**This is the recommended login method for most users.** For more info, see [Open OnDemand](ondemand.md).

## SSH Connection

### Web-based

[Open OnDemand](ondemand.md#command-line-terminal) includes a command-line terminal app that connects you to the cluster via SSH. This is the simplest method for users connecting from Windows computers. However, this method does not account for X11. If you plan to run windowed applications, please see the [X11](#using-ssh-with-x11) guide below.

### From Windows

Windows users will need a SSH client application installed on the desktop. Research Computing recommends [MobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html) which includes a terminal emulator, file browser, and X11 server. [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) is another option and can be made available in Citrix.

### From Mac or Linux

Mac and Linux users also need a SSH client but benefit from built-in options within the operating system. Additional options exist that will include more features. For example, the [iTerm2](https://www.iterm2.com/) application for Mac users is an alternative to the built-in Terminal app.

## Using SSH to login

Each SSH client will vary in specific detail but all will ask for a **hostname**, **username**, and **password**.

### Hostname

Each login hostname follows a common naming convention, ***login-clustername.rcc.mcw.edu***.

Depending on your permissions, you may login to the following clusters:

- **login-hpc.rcc.mcw.edu** - primary SLURM cluster

### Username

When asked for your username, please use your MCW NetID, which is used for most MCW resources including email login.

### Password

When prompted, enter your MCW password, which is used for most MCW resources including email login.

!!! danger "Never share your password"
    MCW policy prohibits sharing of passwords. Research Computing staff never has access to your password, and will never ask you for your password.

### Example login

In a command-line session:

```bash
ssh netid@login-clustername.rcc.mcw.edu
```

!!! tip "Hidden Password"
    Most SSH client applications will hide your password as you type. This is a security feature, not an error.

## Using SSH with X11

X11 allows you to run a windowed application on a remote server and transmit the graphical view back to your computer. This is helpful if you want to run a graphical application that is installed on the cluster, but do not want to use a compute node or go through Open OnDemand. Mac users will need Xquartz installed. Windows users should use [MobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html).

!!! warning "User experience"
    Running a windowed application via SSH with X11 can be a very slow, and sometimes unusable experience. When possible, avoid using X11 over VPN or WiFi.

To login to HPC cluster with X11 enabled:

```bash
ssh -Y netid@login-hpc.rcc.mcw.edu
```

Your SSH login will proceed as normal. To launch a windowed application, load the correct module and start the software. The graphical view of the application will open on your computer.

## Additional SSH options

OpenSSH has many configuration options that you can use to customize your login experience. Custom SSH options are added to `~/.ssh/config` on your local computer.

### Custom hostnames

You can customize and simplify the login hostnames with a host entry in `~/.ssh/config`.

```txt
Host login-hpc
    HostName login-hpc.rcc.mcw.edu
    User netid
```

This will simplify:

`ssh netid@login-hpc.rcc.mcw.edu`

to:

`ssh login-hpc`
