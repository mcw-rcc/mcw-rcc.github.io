# Schrodinger

!!! warning "RCC will no longer centrally fund and purchase Schrodinger" 
    Schrodinger will continue to be free to all users during the current license term 01/17/2022-01/16/2023. To continue using Schrodinger after 01/16/2023, existing users will have to acquire their own license. Please email {{ support_email}} with questions/concerns.

RCC has licensed the Small-Molecule Drug Discovery Suite by [Schrodinger](https://www.schrodinger.com/suites/small-molecule-drug-discovery-suite). The software includes a GUI client that you can run on your Windows, Mac, or Linux desktop/laptop. This client can be configured to send jobs to the new HPC Cluster. Example use includes molecular modeling, docking, molecular dynamics simulation, etc. See below for installation.

## Requirements
* RCC user account

## Installation & Configuration
1. Download 2022-1 software from <https://www.schrodinger.com> and run the installer.
2. Locate the schrodinger.hosts file.
: !!! tip "Windows"
    `$INSTALLDIR\Schrodinger 2022-1\schrodinger.hosts`
: !!! tip "Mac"
    `/opt/schrodinger/suites2022-1/schrodinger.hosts`

3. Download the [server schrodinger.hosts file](https://mcw.box.com/shared/static/aa5gq2ndgejsn8htvyvshpehbjf4jvjz.hosts). Add the text from the downloaded file to your schrodinger.hosts file (you located in step #2). Replace **NetID** with your MCW username and save the file.

4. Setup remote connection to the cluster.
: !!! tip "Windows"
    Open the **Remote Login Configuration** tool. Select **Generate Keys**. Select **Initialize Host Access**. Enter the host IP Address (this is in your schrodinger.hosts file) and your MCW username. Select **Initialize**.
: !!! tip "Mac"
    Configure password-less SSH for remote login. Open a terminal and follow [this guide](http://www.linuxproblem.org/art_9.html).

5. Contact {{ support_email }} for license information. Then open the **Configure Licensing** tool. Select **I can identify my license server** from the **Add Licenses** drop-down menu. Enter the Host Name and Port provided by Research Computing. Click Save Server.

6. Launch the Maestro application.

## Upgrading
Upgrading your Schrodinger installation is easy and can be done in two steps using the included Uninstall programs for your platform. After you uninstall Schrodinger, install the new version. This process should find all of the previous configuration. You will need to update your schrodinger.hosts file as it appears above. If you have issues upgrading, please contact {{ support_email }}.

## License
The RCC Schrodinger license includes 25 tokens for the Small-Molecule Drug Discovery Suite. An additional 20 tokens are available for the Glide docking program. Each job requires a number of tokens for each program in use. Please see the following guide for token usage information.

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
$ /hpc/apps/schrodinger/2022-1/run script.py -h
```

To run the **script** in a job on the cluster, adapt the following job submission script to your specific command:
=== "schrod.slurm"
```bash
#!/bin/bash
#SBATCH --job-name=test_job
#SBATCH --ntasks=1
#SBATCH --time=01:00:00
#SBATCH --output=%x-%j.out

/hpc/apps/schrodinger/2022-1/run script.py
```

## Troubleshooting
Schrodinger can be sensitive to changes in networking. This is often an issue for laptop users. You may see errors such as:

* Launch failed: no JobId found.
* WARNING: You did not specify for -maxjob. Remember its default value is 1.

In this case, you should reset your connection to the Schrodinger server using the following steps.

!!! warning "Mac OS X only"
    This solution works on Mac OS X and assumes that you have installed Schrodinger 2022-1. Modify the version number if you have an older version installed.

On your laptop/desktop:

* Shutdown Schrodinger.
* Open terminal and run the following commands:
: 
```bash
/opt/schrodinger/suites2022-1/utilities/jserver -shutdown
/opt/schrodinger/suites2022-1/utilities/jserver -cleanall
/opt/schrodinger/suites2022-1/utilities/jserver -proxy -shutdown
/opt/schrodinger/suites2022-1/utilities/jserver -proxy -cleanall
```

On the cluster:

* Run the following commands:
: 
```bash
/hpc/apps/schrodinger/2022-1/utilities/jserver -shutdown
/hpc/apps/schrodinger/2022-1/utilities/jserver -cleanall
/hpc/apps/schrodinger/2022-1/utilities/jserver -proxy -shutdown
/hpc/apps/schrodinger/2022-1/utilities/jserver -proxy -cleanall
```

## Help
Having issues installing? Contact {{ support_email }} to schedule a support session. 

For general Schrodinger questions, see <https://www.schrodinger.com/kb>.

For training opportunities, see <https://www.schrodinger.com/seminars/current> and <https://www.schrodinger.com/training>.

--8<-- "includes/abbreviations.md"