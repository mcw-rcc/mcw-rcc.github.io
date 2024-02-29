# Logging in

ResHPC access requires a project user account, which is based on your MCW NetID and password. Your project username will be a combination of your NetID and a project identifier (example, **jsmith.p1234**). Your password and Duo MFA credentials are the same that you use to access Citrix and other MCW resources.

!!! warning "Network Access"
    ResHPC is available from **Citrix** and **MCW-managed machines**.

## ResHPC OnDemand

If you're new to using a cluster, ResHPC OnDemand offers web browser-based access. You can manage files and start a remote desktop session on the cluster. All of this is possible without logging in via a traditional SSH terminal.

To login, point your browser to <https://ood-reshpc.rcc.mcw.edu>, enter your password, select a Duo MFA option, and complete the login.

!!! tip "Additional info"
    Much of the functionality of ResHPC OnDemand is shared with the standard Open OnDemand site. Please consult the [OnDemand app guides](../user-guide/access/ondemand.md#command-line-terminal) for additional help and detail.

## SSH Connection

ResHPC login requires Duo MFA. For Windows users, we recommend a multi-factor compatible terminal client such as [MobaXterm](../user-guide/access/mobaxterm.md). Mac and Linux users may use their OS provided terminal clients.

To login, use the ResHPC login hostname `login-reshpc.rcc.mcw.edu`, enter your password, select a Duo MFA option, and complete the login process.

Example login flow:

```txt
$ ssh jsmith.p1234@login-reshpc.rcc.mcw.edu
(jsmith.p1234@login-reshpc.rcc.mcw.edu) Password:
(jsmith.p1234@login-reshpc.rcc.mcw.edu) Duo two-factor login for jsmith

Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-XXXX
 2. Phone call to XXX-XXX-XXXX
 3. SMS passcodes to XXX-XXX-XXXX

Passcode or option (1-3):
```
