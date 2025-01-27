# Mounting Drives

!!! warning "SMB access is not enabled by default."

    Before mounting an SMB drive, you must request SMB access to your designated storage space by contacting {{ support_email }}. Please note that if you are granted SMB access to a specific folder, you will no longer be able to access that folder through SSH or OnDemand due to differences in file paths and permissions between the cluster's Linux environment and SMB protocols.

## Mount a SMB Drive

### Windows

Microsoft provides a helpful [guide](https://support.microsoft.com/en-us/windows/map-a-network-drive-in-windows-29ce55d1-34e3-a7e2-4801-131475f9557d){:target="_blank"}. In step #4, please make sure to type in your drive path, which will be in the form `\\qfs2.rcc.mcw.edu\pi_netid`. Do not select **Browse**. Use your MCW username and password when prompted. If you are not on an MCW-owned or managed machine, please make sure to preface your MCW username with `mcwcorp\`.

If you are having trouble remounting your drive, please first right-click on the drive and select disconnect. If the drive disconnects successfully, then try to remount, but make sure to select **Connect using different credentials**. Please contact {{ support_email }} if the issue persists.

### MacOS

In the Finder menu, select `Go` and `Connect to Server`. In the Server Address box, enter your drive path, which will be in the form `smb://qfs2.rcc.mcw.edu/pi_netid`. You will be prompted for your MCW username and password when you connect. Make sure to select `Registered User` and preface your MCW username with `mcwcorp\`.
