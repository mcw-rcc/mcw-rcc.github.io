# SSH Connection

Traditional SSH connection is available for all clusters. However, we do recommend to use [Open OnDemand](ondemand.md) if possible.

## SSH clients

- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) - simplest SSH client for Windows and requires no install
- Mac Terminal App - built-in terminal app available on all Mac computers
- [Iterm2](https://www.iterm2.com/) - enhanced terminal app for Mac computers
- Linux terminal - built-in terminal apps are available in most Linux OS

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

## SSH options

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
