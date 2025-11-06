---
version: 2025-3
version_future: 2025-3 or newer
---
# Schrödinger

!!! info "Schrödinger is licensed by the Department of Biochemistry and available to all MCW investigators."
    Users interested in contributing funds or discussing Schrödinger licensing at MCW should contact [Dawn Wenzel](mailto://dwenzel@mcw.edu) and [Brian Smith](mailto:brismith@mcw.edu).

The [Schrödinger Small-Molecule Drug Discovery Suite](https://www.schrodinger.com/suites/small-molecule-drug-discovery-suite){:target="_blank"} includes a GUI client that you can run on your Windows, Mac, or Linux desktop/laptop. This client can be configured to send jobs to the HPC Cluster. Example use includes molecular modeling, docking, molecular dynamics simulation, etc. See below for installation.

## Requirements

* RCC user account

## Installation & Configuration

!!! warning "Installation Changes"
    Please note, starting with version {{ version }}, the `schrodinger.hosts` file is no longer used to connect to the cluster. This has been replaced by the Schrödinger Job Server. The steps below have been updated to reflect these changes. Please plan to update your Schrodinger to {{ version }}.

### Download and Install

Create a new account at <https://www.schrodinger.com>{:target="_blank"}. You will receive an email to verify your account before you can log in.

Download the {{ version }} software from the same page under the Platform menu. Then, run the installer.

### Configure licensing

=== "Windows"

    Open the **Configure Software** tool and select **Server Identifiers**. Enter the license server hostname and port number found in the [configuration info](https://mcw0.sharepoint.com/:o:/s/RCCAdminSite/EmJ7D-fDCv1Dg0f_Z-_d0tsBR8_trGnDiqZaod6mUPjo8A?e=GdWCGP){:target="_blank"} and select **Install License**.

=== "Mac"

    Go to the your Applications and open the SchrodingerSuites{{ version }} folder. Then, open the **Configuration** tool and select the **Server Identifiers** tab. Enter the license server hostname and port number found in the [configuration info](https://mcw0.sharepoint.com/:o:/s/RCCAdminSite/EmJ7D-fDCv1Dg0f_Z-_d0tsBR8_trGnDiqZaod6mUPjo8A?e=GdWCGP){:target="_blank"} and select **Install License**.

### Register Job Server

Launch the Maestro application. Click the button `Continue to Maestro`. Select `Help > Register with Job Servers...`. Select `New server` and enter the job server hostname and port number found in the [configuration info](https://mcw0.sharepoint.com/:o:/s/RCCAdminSite/EmJ7D-fDCv1Dg0f_Z-_d0tsBR8_trGnDiqZaod6mUPjo8A?e=GdWCGP){:target="_blank"}. Enter your MCW username and password, and select `Register`. If you get a warning about the server being already registered, it can be ignored. Restart Maestro to complete the setup.

## Upgrading

To upgrade your Schrödinger installation, first uninstall the current version. The uninstall process should leave your configuration intact. Then install the upgraded version of Schrödinger and remember to update your schrodinger.hosts file as it appears above. If you have issues upgrading, please contact {{ support_email }}.

## Remote Job Submission

Included in your install are configurations for the remote server connection to HPC Cluster. This allows you to send large or computationally intensive workloads to the cluster from your desktop. The cluster will run the job and return the results.

: **server_cpu** - HPC Cluster connection for Schrödinger CPU jobs
: **server_gpu** - HPC Cluster connection for Schrödinger GPU jobs

When running Schrödinger jobs, please follow these guidelines:

* Run all pre-processing jobs on the localhost (your machine).

* Run docking, MD, etc. jobs with remote connection to HPC Cluster.

## Direct Job Submission

You can also submit jobs directly from the cluster command line. This is helpful for submitting large numbers of jobs, where using the desktop launch interface might be repetitive. All direct jobs are launched from a cluster login node using Schrödinger to submit the job to the SLURM job scheduler. Please follow this syntax using the `server_cpu` or `server_gpu` for `-HOST` as appropriate.

<!-- markdownlint-disable MD046 -->
```bash
# print the command-line options for Desmond (molecular dynamics package)
/hpc/apps/schrodinger/{{ version }}/desmond -h

# submit a desmond job to the SLURM job scheduler
/hpc/apps/schrodinger/{{ version }}/desmond -HOST server_gpu -c x.cfg -in x.cms
```

!!! warning "Mistakes to avoid"
    - Do not run the Schrödinger apps in a job script. Schrödinger will submit the job to SLURM for you.
    - Do not use `localhost` as the `-HOST` setting. This will cause the job to run on the login node and/or bypass the license checking. Either case will cause your job and other jobs to fail.

## Schrödinger Utility Scripts

Schrödinger includes additional utility scripts that are not included in the Maestro interface and must be run directly within a job script. See [Schrödinger scripts](https://www.schrodinger.com/scriptcenter){:target="_blank"} for a complete list and details.

!!! info "Why are utility scripts submitted via job script?"
    These utility scripts do not have the advanced job submission options that the Schrödinger apps (Desmond, Epik, etc.) include. Therefore, we have to write a separate SLURM job script for submission.

To see the options for a particular script on the cluster:

```bash
# script has no requirements
/hpc/apps/schrodinger/{{ version }}/run entropy_calc.py -h
```

Some scripts are dependent on a specific Schrödinger app. Check the [Schrödinger scripts](https://www.schrodinger.com/scriptcenter){:target="_blank"} page **Requires** column.

```bash
# run a script requiring the Desmond app
/hpc/apps/schrodinger/{{ version }}/run -FROM desmond trj_center.py -h
```

To run the **script** in a job on the cluster, adapt the following job submission script to your specific command:
=== "schrod.slurm"

```bash
#!/bin/bash
#SBATCH --job-name=test_job
#SBATCH --ntasks=1
#SBATCH --time=01:00:00
#SBATCH --output=%x-%j.out

/hpc/apps/schrodinger/{{ version }}/run script.py
```

## Troubleshooting

Schrödinger can be sensitive to changes in networking. This is often an issue for laptop users. You may see errors such as:

* Launch failed: no JobId found.
* WARNING: You did not specify for -maxjob. Remember its default value is 1.

In this case, you should reset your connection to the Schrödinger server using the following steps.

!!! warning "Mac OS X only"
    This solution works on Mac OS X and assumes that you have installed Schrödinger {{ version }}. Modify the version number if you have an older version installed.

On your laptop/desktop:

* Shutdown Schrödinger.
* Open terminal and run the following commands:

```bash
/opt/schrodinger/suites{{ version }}/utilities/jserver -shutdown
/opt/schrodinger/suites{{ version }}/utilities/jserver -cleanall
/opt/schrodinger/suites{{ version }}/utilities/jserver -proxy -shutdown
/opt/schrodinger/suites{{ version }}/utilities/jserver -proxy -cleanall
```

On the cluster:

* Run the following commands:

```bash
/hpc/apps/schrodinger/{{ version }}/utilities/jserver -shutdown
/hpc/apps/schrodinger/{{ version }}/utilities/jserver -cleanall
/hpc/apps/schrodinger/{{ version }}/utilities/jserver -proxy -shutdown
/hpc/apps/schrodinger/{{ version }}/utilities/jserver -proxy -cleanall
```

## Help

Having issues installing? Contact {{ support_email }} to schedule a support session.

For general Schrödinger questions, see <https://www.schrodinger.com/kb>{:target="_blank"}.

For training opportunities, see <https://www.schrodinger.com/seminars/current>{:target="_blank"} and <https://www.schrodinger.com/training>{:target="_blank"}.

--8<-- "includes/abbreviations.md"
