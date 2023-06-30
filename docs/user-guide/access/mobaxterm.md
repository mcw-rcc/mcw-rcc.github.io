# MobaXterm

MobaXterm is a full-featured login tool for Windows users. It features a pseudo-shell, SSH client, SFTP client, and X11 server. Unlike other programs that handle one or two of these features, MobaXterm bundles all features in an easy-to-use format. Lastly, it doesn't require installation to run.

## Downloading

MobaXterm is available at the link below. Select the Installer version and proceed with normal Windows style install. Select the Portable version if you want to run without installing.

[Download MobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html){:target="_blank" .md-button .md-button--primary }

If you downloaded the portable version, make sure to save it in a place that is easy to access and will not be deleted.

## Cluster Login

Start MobaXterm and select `Sessions > New Session` from the top menu. This will open the `Session Settings` window. Select `SSH`, fill in the remote host box with **login-hpc.rcc.mcw.edu**, and select `Ok`.

You will see a terminal prompt `login as:`. Type in your MCW username and press enter. You will see a prompt for your password. Type in your MCW password and press enter. Please note that the password is hidden as you type for security purposes.

## File Transfer

The built-in SFTP client will start and load on the left side of the MobaXterm window. The default location is your cluster home directory. However, all of the folders that you normally access from the command-line are available.

## X11 Windowed Apps

MobaXterm includes a X11 server that allows you to run windowed applications that are installed on the cluster, but have them appear on your desktop. While this is convenient, it can be very slow for resource intensive applications, especially if you are on VPN. For resource intensive windowed apps you should try the Remote Desktop app in Open OnDemand.

### Example - IGV

To launch IGV, first login to the cluster as described above and load the IGV module.

```bash
module load igv
```

Then, launch IGV.

```bash
igv
```

IGV should open in a new window on your desktop.
