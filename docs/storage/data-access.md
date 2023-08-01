# Mounting Drives

Research Computing group storage is also available via an SMB mount on your Windows or Mac desktop. This access is convenient, but cannot be used to access files that you have saved via your cluster access. In other words, you can save and utilize data via your cluster access (Linux), or via this access (Windows), but the data cannot be accessed from both.

Contact {{ support_email }} to request Windows access to your storage space.

## Mount a Windows Drive

Microsoft provides a helpful [guide](https://support.microsoft.com/en-us/windows/map-a-network-drive-in-windows-29ce55d1-34e3-a7e2-4801-131475f9557d). In step #4, please make sure to type in your drive path, which will be in the form `\\qfs2.rcc.mcw.edu\pi_netid`. Do not select **Browse**. Use your MCW username and password when prompted.If you are not on an MCW-owned or managed machine, please make sure to preface your MCW username with `mcwcorp\`.

If you are having trouble remounting your drive, please first right-click on the drive and select disconnect. If the drive disconnects successfully, then try to remount, but make sure to select **Connect using different credentials**. Please contact {{ support_email }} if the issue persists.
