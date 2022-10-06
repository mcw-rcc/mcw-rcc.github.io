# Logging in

ResHPC access requires a project user account, which is based on your MCW NetID and password. Your project username will be a combination of your NetID and a project identifier (example, **jsmith.p12345**). Your password and Duo MFA credentials are the same that you use to access Citrix and other MCW resources.

!!! warning "Network Access"
    ResHPC is available from **Citrix** and **MCW-managed machines**.

ResHPC login requires Duo MFA. For Windows users, we recommend a multi-factor compatible terminal client such as [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). Mac and Linux users may use their OS provided terminal clients.

To login, use the ResHPC login hostname `login-reshpc.rcc.mcw.edu`, enter your password, select a Duo MFA option, and complete the login process.

Example login flow:

```txt
$ ssh jsmith.p12345@login-reshpc.rcc.mcw.edu
(jsmith.p12345@login-reshpc.rcc.mcw.edu) Password:
(jsmith.p12345@login-reshpc.rcc.mcw.edu) Duo two-factor login for jsmith

Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-XXXX
 2. Phone call to XXX-XXX-XXXX
 3. SMS passcodes to XXX-XXX-XXXX

Passcode or option (1-3):
```
<!--
SRCE provides two access methods. Both methods require password authentication with Duo MFA.
: **SRCE OnDemand** provides a graphical interface similar to a remote desktop.
: **SSH** provides command-line access.

!!! warning "Network Access"
    SRCE is available from Citrix and MCW-managed machines.

## SRCE OnDemand

If you're new to using a cluster, SRCE OnDemand offers web browser-based access. You can manage files and start a remote desktop session on the cluster. All of this is possible without logging in via a traditional SSH terminal.

[Login to SRCE OnDemand](https://srce.rcc.mcw.edu/){ .md-button .md-button--primary }

For more info, see [SRCE OnDemand](ondemand.md#connecting-to-open-ondemand).

## SSH

SSH login from a Windows machine requires a multi-factor compatible terminal client. We recommend [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). Mac and Linux have built-in terminal clients.

To login, use the SRCE login hostname `login-srce.rcc.mcw.edu`, enter your project password, select a Duo MFA option, and complete the login process.

Example login to SRCE from a MacOS Terminal App:

```bash
$ ssh jsmith.p12345@login-srce.rcc.mcw.edu
(jsmith.p12345@login-srce.rcc.mcw.edu) Password:
(jsmith.p12345@login-srce.rcc.mcw.edu) Duo two-factor login for jsmith

Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-XXXX
 2. Phone call to XXX-XXX-XXXX
 3. SMS passcodes to XXX-XXX-XXXX

Passcode or option (1-3):
```
-->
