# MobaXterm

MobaXterm is a full-featured login tool for Windows users. It features a pseudo-shell, SSH client, SFTP client, and X11 server. Unlike other programs that handle one or two of these features, MobaXterm bundles all features in an easy-to-use format. Lastly, it doesn't require installation to run.

## Downloading

MobaXterm is available at the link below. Select the Installer version and proceed with normal Windows style install. Select the Portable version if you want to run without installing.

[Download MobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html){:target="_blank" .md-button .md-button--primary }

If you downloaded the portable version, make sure to save it in a place that is easy to access and will not be deleted.

## Cluster Login

To get started, start MobaXterm and select `Session > SSH`. In the session settings, enter `login-hpc.rcc.mcw.edu` for the **Remote host**, and enter your MCW username for the **Username**. To login, select `Ok`.

![MobaXterm SSH](../../_static/img/MobaXterm-SSH.png){ width="600" }

You will see a prompt for your password. Type in your MCW password and press enter. Please note that the password is hidden or security purposes.

!!! tip "Local terminal"
    Alternatively, you may select **Start local terminal** on the MobaXterm startup screen. This will create a local terminal on your Windows computer, which you could use to login to the cluster via traditional SSH commands.

## File Transfer

The built-in SFTP client will start and load on the left side of the MobaXterm window when you login to a remote host. This will show you your files on the remote host. In the case of the cluster, it will show your cluster home directory. Additionally, all of the folders that you normally access from the cluster command-line are available.

MobaXterm also has a standalone SFTP client application. To start the SFTP client, select `Session > SFTP` and fill in the **Remote host** and **Username**.

### Multi-factor Authentication

To transfer data to secure systems such as ResHPC, you will need to enable multi-factor authentication (MFA) for the MobaXterm SFTP client. In non-CLI cases such as MobaXterm SFTP, the dialogue interface for MFA requires additional configuration. To enable MFA for SFTP using MobaXterm, select `Session > SFTP > Advanced Sftp settings`. Enter the **Remote host**, **Username**, and make sure the **2-steps authentication** box is checked.

![MobaXterm SFTP 2FA](../../_static/img/MobaXterm-SFTP-2FA.png){ width="600" }

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

## SSH Keys

Please see the MobaXterm section in the [SSH keys guide](ssh-keys.md#mobaxterm-cli){:target="_blank"}.
