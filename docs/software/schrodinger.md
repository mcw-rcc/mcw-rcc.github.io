# Schrodinger

!!! info "Schrodinger is licensed by the Department of Biochemistry and available to all MCW investigators."
    Users interested in contributing funds or discussing Schrodinger licensing at MCW should contact Dawn Wenzel (dwenzel@mcw.edu) and Brian Smith (brismith@mcw.edu).

The [Schrodinger Small-Molecule Drug Discovery Suite](https://www.schrodinger.com/suites/small-molecule-drug-discovery-suite) includes a GUI client that you can run on your Windows, Mac, or Linux desktop/laptop. This client can be configured to send jobs to the HPC Cluster. Example use includes molecular modeling, docking, molecular dynamics simulation, etc. See below for installation.

## Requirements

* RCC user account

## Installation & Configuration

1. Download 2022-4 software from <https://www.schrodinger.com> and run the installer.
2. Locate the schrodinger.hosts file.
: **Windows**  
    `$INSTALLDIR\Schrodinger 2022-4\schrodinger.hosts`
: **Mac**  
    `/opt/schrodinger/suites2022-4/schrodinger.hosts`

3. Download the [server schrodinger.hosts file](https://mcw.box.com/s/cc1rafcj0cvhp54d6d7prhl7gw2rb55g). Add the text from the downloaded file to your schrodinger.hosts file (you located in step #2). Replace **NetID** with your MCW username and save the file.

4. Open the **Configure Licensing** tool and select **I can identify my license server** from the **Add Licenses** drop-down menu. Enter `lic01.rcc.mcw.edu` as the Host Name and `27008` as the Port. Click **Save Server**. *If you see a remote license server warning, this can be ignored.*

    !!! warning
        The license server hostname has changed. If you used Schrodinger prior to 2023, you will need to update your client with the new license information.

5. Setup remote connection to the cluster.
: **Windows**  
    Open the `Remote Login Configuration` tool. Select `Generate Keys`. Select `Initialize Host Access`. Enter the host IP Address (this is in your schrodinger.hosts file) and your MCW username. Select `Initialize`.
: **Mac**  
    Configure password-less SSH for remote login. Open a terminal and follow [this guide](http://www.linuxproblem.org/art_9.html).

6. Launch the Maestro application.

## Upgrading

To upgrade your Schrodinger installation, first uninstall the current version. The uninstall process should leave your configuration intact. Then install the upgraded version of Schrodinger and remember to update your schrodinger.hosts file as it appears above. If you have issues upgrading, please contact {{ support_email }}.

## Usage

Included in your install are configurations for the remote server connection to HPC Cluster. These settings are used as follows:

: **server_cpu** - HPC Cluster connection for Schrodinger CPU jobs
: **server_gpu** - HPC Cluster connection for Schrodinger GPU jobs

When running Schrodinger jobs, please follow these guidelines:

* Run all pre-processing jobs on the localhost (your machine).

* Run docking, MD, etc. jobs with remote connection to HPC Cluster.

## Schrodinger Command-Line

Some Schrodinger scripts are not included in the Maestro interface and must be run from the command line. See [Schrodinger scripts](https://www.schrodinger.com/scriptcenter) for a complete list and details.

To see the options for a particular **script.py** on the cluster:

```bash
/hpc/apps/schrodinger/2022-4/run script.py -h
```

To run the **script** in a job on the cluster, adapt the following job submission script to your specific command:
=== "schrod.slurm"

```bash
#!/bin/bash
#SBATCH --job-name=test_job
#SBATCH --ntasks=1
#SBATCH --time=01:00:00
#SBATCH --output=%x-%j.out

/hpc/apps/schrodinger/2022-4/run script.py
```

## Troubleshooting

Schrodinger can be sensitive to changes in networking. This is often an issue for laptop users. You may see errors such as:

* Launch failed: no JobId found.
* WARNING: You did not specify for -maxjob. Remember its default value is 1.

In this case, you should reset your connection to the Schrodinger server using the following steps.

!!! warning "Mac OS X only"
    This solution works on Mac OS X and assumes that you have installed Schrodinger 2022-4. Modify the version number if you have an older version installed.

On your laptop/desktop:

* Shutdown Schrodinger.
* Open terminal and run the following commands:

```bash
/opt/schrodinger/suites2022-4/utilities/jserver -shutdown
/opt/schrodinger/suites2022-4/utilities/jserver -cleanall
/opt/schrodinger/suites2022-4/utilities/jserver -proxy -shutdown
/opt/schrodinger/suites2022-4/utilities/jserver -proxy -cleanall
```

On the cluster:

* Run the following commands:

```bash
/hpc/apps/schrodinger/2022-4/utilities/jserver -shutdown
/hpc/apps/schrodinger/2022-4/utilities/jserver -cleanall
/hpc/apps/schrodinger/2022-4/utilities/jserver -proxy -shutdown
/hpc/apps/schrodinger/2022-4/utilities/jserver -proxy -cleanall
```

## Help

Having issues installing? Contact {{ support_email }} to schedule a support session.

For general Schrodinger questions, see <https://www.schrodinger.com/kb>.

For training opportunities, see <https://www.schrodinger.com/seminars/current> and <https://www.schrodinger.com/training>.

--8<-- "includes/abbreviations.md"
