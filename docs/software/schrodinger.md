# Schrodinger

!!! info "Schrodinger is licensed by the Department of Biochemistry and available to all MCW investigators."
    Users interested in contributing funds or discussing Schrodinger licensing at MCW should contact [Dawn Wenzel](mailto://dwenzel@mcw.edu) and [Brian Smith](mailto:brismith@mcw.edu).

The [Schrodinger Small-Molecule Drug Discovery Suite](https://www.schrodinger.com/suites/small-molecule-drug-discovery-suite){:target="_blank"} includes a GUI client that you can run on your Windows, Mac, or Linux desktop/laptop. This client can be configured to send jobs to the HPC Cluster. Example use includes molecular modeling, docking, molecular dynamics simulation, etc. See below for installation.

## Requirements

* RCC user account

## Installation & Configuration

1. Download 2024-1 software from <https://www.schrodinger.com>{:target="_blank"} and run the installer.
2. Locate the schrodinger.hosts file.
: **Windows**  
    `$INSTALLDIR\Schrodinger 2024-1\schrodinger.hosts`
: **Mac**  
    `/opt/schrodinger/suites2024-1/schrodinger.hosts`

3. Download the [server schrodinger.hosts file](https://mcw0.sharepoint.com/:u:/s/RCCAdminSite/EY4HbvHa8p9JkL45LallbpoBlUW89HifDwvcLRocyAVBIA?e=fjgl3N){:target="_blank"}. Add the text from the downloaded file to your schrodinger.hosts file (you located in step #2). Replace **NetID** with your MCW username and save the file.

4. Open the **Configure Licensing** tool and select **I can identify my license server** from the **Add Licenses** drop-down menu. Locate the hostname and port in the [licensing info](https://mcw0.sharepoint.com/:o:/s/RCCAdminSite/EmJ7D-fDCv1Dg0f_Z-_d0tsBR8_trGnDiqZaod6mUPjo8A?e=GdWCGP){:target="_blank"}. Click **Save Server**. *If you see a remote license server warning, this can be ignored.*

    !!! warning
        The license server hostname has changed. If you used Schrodinger prior to 2023, you will need to update your client with the new license information.

5. Setup remote connection to the cluster.
: **Windows**  
    Open the `Remote Login Configuration` tool. Select `Generate Keys`. Select `Initialize Host Access`. Enter the host IP Address (this is in your schrodinger.hosts file) and your MCW username. Select `Initialize`.
    !!! warning "Known issue"
        The 2024-1 Remote Login Configuration tool may error due to missing files. This is a known issue. To work around this issue, launch the `Schrodinger Command Prompt` and run command `run SchrodingerPlinkWizard`. Contact {{ support_email }} with questions.
: **Mac**  
    Configure password-less SSH for remote login. Open a terminal and follow [this guide](http://www.linuxproblem.org/art_9.html){:target="_blank"}.

6. Launch the Maestro application.

## Upgrading

To upgrade your Schrodinger installation, first uninstall the current version. The uninstall process should leave your configuration intact. Then install the upgraded version of Schrodinger and remember to update your schrodinger.hosts file as it appears above. If you have issues upgrading, please contact {{ support_email }}.

## Remote Job Submission

Included in your install are configurations for the remote server connection to HPC Cluster. This allows you to send large or computationally intensive workloads to the cluster from your desktop. The cluster will run the job and return the results.

: **server_cpu** - HPC Cluster connection for Schrodinger CPU jobs
: **server_gpu** - HPC Cluster connection for Schrodinger GPU jobs

When running Schrodinger jobs, please follow these guidelines:

* Run all pre-processing jobs on the localhost (your machine).

* Run docking, MD, etc. jobs with remote connection to HPC Cluster.

## Direct Job Submission

You can also submit jobs directly from the cluster command line. This is helpful for submitting large numbers of jobs, where using the desktop launch interface might be repetitive. All direct jobs are launched from a cluster login node using Schrodinger to submit the job to the SLURM job scheduler. Please follow this syntax using the `server_cpu` or `server_gpu` for `-HOST` as appropriate.

```bash
# print the command-line options for Desmond (molecular dynamics package)
/hpc/apps/schrodinger/2024-1/desmond -h

# submit a desmond job to the SLURM job scheduler
/hpc/apps/schrodinger/2024-1/desmond -HOST server_gpu -c x.cfg -in x.cms
```

!!! warning "Mistakes to avoid"
    - Do not run the Schrodinger apps in a job script. Schrodinger will submit the job to SLURM for you.
    - Do not use `localhost` as the `-HOST` setting. This will cause the job to run on the login node and/or bypass the license checking. Either case will cause your job and other jobs to fail.

## Schrodinger Utility Scripts

Schrodinger includes additional utility scripts that are not included in the Maestro interface and must be run directly within a job script. See [Schrodinger scripts](https://www.schrodinger.com/scriptcenter){:target="_blank"} for a complete list and details.

!!! info "Why are utility scripts submitted via job script?"
    These utility scripts do not have the advanced job submission options that the Schrodinger apps (Desmond, Epik, etc.) include. Therefore, we have to write a separate SLURM job script for submission.

To see the options for a particular script on the cluster:

```bash
# script has no requirements
/hpc/apps/schrodinger/2024-1/run entropy_calc.py -h
```

Some scripts are dependent on a specific Schrodinger app. Check the [Schrodinger scripts](https://www.schrodinger.com/scriptcenter){:target="_blank"} page **Requires** column.

```bash
# run a script requiring the Desmond app
/hpc/apps/schrodinger/2024-1/run -FROM desmond trj_center.py -h
```

To run the **script** in a job on the cluster, adapt the following job submission script to your specific command:
=== "schrod.slurm"

```bash
#!/bin/bash
#SBATCH --job-name=test_job
#SBATCH --ntasks=1
#SBATCH --time=01:00:00
#SBATCH --output=%x-%j.out

/hpc/apps/schrodinger/2024-1/run script.py
```

## Troubleshooting

Schrodinger can be sensitive to changes in networking. This is often an issue for laptop users. You may see errors such as:

* Launch failed: no JobId found.
* WARNING: You did not specify for -maxjob. Remember its default value is 1.

In this case, you should reset your connection to the Schrodinger server using the following steps.

!!! warning "Mac OS X only"
    This solution works on Mac OS X and assumes that you have installed Schrodinger 2024-1. Modify the version number if you have an older version installed.

On your laptop/desktop:

* Shutdown Schrodinger.
* Open terminal and run the following commands:

```bash
/opt/schrodinger/suites2024-1/utilities/jserver -shutdown
/opt/schrodinger/suites2024-1/utilities/jserver -cleanall
/opt/schrodinger/suites2024-1/utilities/jserver -proxy -shutdown
/opt/schrodinger/suites2024-1/utilities/jserver -proxy -cleanall
```

On the cluster:

* Run the following commands:

```bash
/hpc/apps/schrodinger/2024-1/utilities/jserver -shutdown
/hpc/apps/schrodinger/2024-1/utilities/jserver -cleanall
/hpc/apps/schrodinger/2024-1/utilities/jserver -proxy -shutdown
/hpc/apps/schrodinger/2024-1/utilities/jserver -proxy -cleanall
```

## Help

Having issues installing? Contact {{ support_email }} to schedule a support session.

For general Schrodinger questions, see <https://www.schrodinger.com/kb>.

For training opportunities, see <https://www.schrodinger.com/seminars/current> and <https://www.schrodinger.com/training>.

--8<-- "includes/abbreviations.md"
