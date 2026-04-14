# Globus

Globus is a secure file transfer tool for research data. It is popular at research institutions and commonly deployed as a server. The unique features of Globus allow file transfer between MCW systems and between MCW and other schools.

!!! info "Globus @ MCW"
    Research Computing has deployed Globus with collections for home, group, and scratch storage. MCW does not have a Globus subscription and certain operations, including sharing, are not permitted.

## Globus Connect Server

MCW Research Computing provides **Globus Connect Server v5 (GCSv5)** mapped collections for fast, secure data transfer to and from RCC storage systems.  
These collections map directly to your RCC directories:

- **Home directories** (`/home/$USER`)
- **Group directories** (`/group/PI/work`)
- **Scratch directories** (`/scratch/g/PI`)

This guide walks you through logging in, finding the MCW RCC collections, and authorizing access the first time you use them.

### Authenticate

Go to <https://app.globus.org>{: target="_blank" }, select **Medical College of Wisconsin** from the organizational login menu, and click **Continue**.

![MCW Organization Login](../_static/img/gcs-login-org.png){ width="500" }

You will be redirected to the MCW login page. Log in using your MCW NetID and password. Once authenticated, Globus will bring you to the **File Manager**.

### Access storage

Inside File Manager, select the **Collection** search bar and type `MCW RCC`. You will see the three mapped collections matching your RCC storage locations on the HPC cluster.

![MCW RCC Collection Search Results](../_static/img/gcs-collection-search.png){ width="700" }

Select a collection to access your data. You may see a message about **Authentication/Consent** (required only for first access). Select **Continue**.

![Consent Required Prompt](../_static/img/gcs-collection-consent.png){ width="600" }

Globus will then ask for permission to (1) Manage data using Globus Transfer and (2) Access the MCW RCC collection on your behalf. Select **Use my MCW email identity**, then select **Allow**.  

![Globus Allow Access](../_static/img/gcs-allow-access.png){ width="600" }

After this, the collection will open normally.

### Transferring data

To begin transferring data, open the [File Manager]<https://app.globus.org/file-manager>{: target="_blank" }. For ease of use, select the middle side-by-side layout in the upper right corner. Use the collection search bars to select a source collection on the left, and a destination collection on the right. Select files and folders for transfer, then select **Start** on the left side, initiating transfer from left to right. To transfer data in the opposite direction, select files and folders for transfer, then select **Start** on the right side, initiating transfer from right to left.

!!! info "Globus Tip"
    Globus continues working even if you close the window or lose your browser connection.

## Globus Connect Personal

Globus Connect Personal runs on your computer and allows data transfer to/from the cluster storage via RCC's Globus Connect Server.

To get started, follow the [Windows](https://docs.globus.org/globus-connect-personal/install/windows/){: target="_blank" }, [Mac](https://docs.globus.org/globus-connect-personal/install/mac/){: target="_blank" }, or [Linux](https://docs.globus.org/globus-connect-personal/install/linux/){: target="_blank" } guide for installation on your computer.

## Help

Please contact {{ support_email }} for all Globus questions.
