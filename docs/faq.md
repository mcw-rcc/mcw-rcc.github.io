# Frequently Asked Questions

## Access & Login

??? question "How do I get an account?"
    Please go to the [Account Request page](https://docs.rcc.mcw.edu/user-guide/accounts/){:target="_blank"} and click "Request an Accout". Fill in the form and we will process your request. We will inform you once your account has been created or if we need further information.
??? question "How do I login?"
    The method you use to login depends on your computer and use case. We suggest you start with the [quickstart guide](user-guide/quickstart.md#logging-in).

??? question "Why can't I login?"
    * You might not have an account.

    * You aren't using your MCW NetID and password to login. Remember that if you are a student, your NetID might be different to your email login.

    * You followed a guide and are using the word ''NetID'' as your username.

??? question "Why don't I have access to my PI's `/group` directory?"
    You might not be correctly affiliated with your PI. Contact {{ support_email }} to correct this issue.

??? question "How do I get access to a collaborator's files?"
    Ask the collaborator PI to contact {{ support_email }} to request the access.

??? question "How do I add a student to my Research Group Storage security group?"
    * PIs should contact {{ support_email }} with the username of your new student.

    * Non-PIs should contact {{ support_email }} with your username, the PI's username, and the username of the new student.

??? question "How do I reset my password?"
    RCC uses the same credentials as MCW's other services. If you need to reset your password, use the [Self Service Password Reset](https://infoscope.mcw.edu/is/services/sspr.htm){:target="_blank"}.

## Job Management

??? question "How do I submit a job to the HPC cluster?"
    You can submit a job to the HPC cluster with the `sbatch` command.

    ```bash
    $ sbatch hpc-run.slurm
    Submitted batch job 6782
    ```

??? question "How do I run an interactive job on the cluster?"
    You can start an interactive job on the HPC cluster using the SLURM `srun` command.

    ```bash
    $ srun --ntasks=1 --mem-per-cpu=4GB --time=01:00:00 --job-name=interactive --pty bash
    ```

??? question "How do I check the status of my job?"
    You can find the current status of your job with the `squeue` command.

    ```bash
    $ squeue -j 6696
                JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
                    1234    normal Testing user  R      37:09      1 cn59
    ```

??? question "How can I see the status of the HPC cluster?"
    You can find the current status of the HPC cluster resources with the `slurminfo` command.

    ```bash
    $ slurminfo
            QUEUE   FREE  TOTAL   FREE  TOTAL   RESORC    OTHER  MAXJOBTIME    CORES    NODE  GPU
        PARTITION  CORES  CORES  NODES  NODES  PENDING  PENDING   DAY-HR:MN  PERNODE  MEM-GB (COUNT)
            normal   1715   2880     34     60        0        0     7-00:00       48     360 -
            bigmem      0     96      0      2        0        0     7-00:00       48    1500 -
            gpu       279    288      3      6        0        0     7-00:00       48     360 gpu:v100(4)
    ```

??? question "Why is my job not running?"
    There are several reasons that your job might not be running.

    * First, the cluster could be busy and your job might be waiting for resources to become available. The cluster is a shared resource and some wait time, while often short or non-existent, can be expected.

    * Your job might be requesting resources that don't exist. Check the output of <nobr>`squeue -j JobID`</nobr>, where ***JobID*** is your SLURM job number. In the final column '''Nodelist (Reason)''' you might see '''PD''' followed by a reason. This could indicate that your job is temporarily waiting for resources (see above) or is blocked.

    * Your job might be blocked by a maintenance window. For details see [Job Scheduling and Maintenance](user-guide/jobs/running-jobs.md#job-scheduling-and-maintenance).

??? question "Can I run a task/script on a login node?"
    Yes, but you should make sure you know exactly how many cores and memory the task will use. Additionally, you should make sure this task will not run for more than **30 min**. Examples of allowed tasks might be a pre- or post- processing task for your input/output files. This might also include compiling or installing software. In general, please try to run all computationally intensive tasks on on the cluster compute nodes. For details see the [User Etiquette Guide](user-guide/etiquette.md).

??? question "Can I have root or sudo access on a compute node?"
    No. RCC does not allow root and/or sudo access on any system.

??? question "What if my job requires more than 7 days? Can I extend my job time?"
    Yes. After you start the job, email the job number and time extension request to {{ support_email }}.

## Software

??? question "What software is available on the HPC cluster?"
    You can list all available software on the cluster with the `module avail` command. See the [cluster software guide](software/modules.md) for details.

??? question "Can I install my own software?"
    Yes, you may install your own software. The process will vary depending on the type of software. Please contact {{ support_email }} if you need assistance.

??? question "Will RCC install my software for me?"
    You may request to have RCC install your software on the cluster. RCC will make a best effort but cannot guarantee that your software will run on the cluster. Please send software installation requests to {{ support_email }}.

??? question "How do I install an R package?"
    You can install your own R packages. See the [R guide](software/R.md) for details. Please also that RCC has centrally installed many packages that are either commonly used, or difficult to install. We suggest to check first if your package is already installed. Please contact {{ support_email }} for assistance or to have RCC install the package for you.

??? question "How do I install a Python package?"
    You can install your own Python packages using the system Python, a local Python, a virtual environment, or a conda environment. See the [Python guide](software/python.md) for details. Please also that RCC has centrally installed many packages that are either commonly used, or difficult to install. We suggest to check first if your package is already installed. Please contact {{ support_email }} for assistance or to have RCC install the package for you.

??? question "Can I use Docker on the cluster?"
    No. Docker is not installed or allowed on RCC systems due to security. However, RCC does provide a container solution called [Singularity](software/containers.md), which can import Docker containers to run on the cluster.

## Data Transfer & Storage

??? question "How do I check my available storage directories and utilization?"
    You can easily find your available storage directories and current utilization with the `mydisks` command.

    ```bash
    $ mydisks
    ======My Lab======
    Size  Used Avail Use% File
    47G   29G   19G  61% /home/user
    932G  158G  774G  17% /group/pi
    4.6T     0  4.6T   0% /scratch/g/pi
    ```

??? question "Why does my quota show as less space?"
    Your computer reports storage utilization in **base-2 math** and the storage system quotas use **base-10 math**. So, if your quota is 1TB, the `mydisks` command will show 932GB. *Please note that this is a difference in mathematical representation of the same value.* While the `mydisks` command shows less space, you are still able to use your full quota.

??? question "What types of storage are available?"
    RCC offers multiple types of storage to all users. Each storage type has a unique path on the cluster, and a unique use-case. Some storage is meant only for use with the cluster, while other storage like [Research Group Storage](storage/paid-storage.md) can be used as generalized Windows storage. Please see the [Storage Overview](storage/rcc-storage.md) for details.

??? question "Can I increase my storage limits?"
    Your home directory limit cannot be increased. You scratch directory limit may be increase upon request but this is subject to availability. Your `/group` Research Group Storage directory may be increased by [purchasing additional space](storage/paid-storage.md).

??? question "Why is `/group` not available on HPC cluster compute nodes?"
    The file system that contains `/group` is not designed for performance computing. In order to preserve every user's experience with `/group`, it is not available on compute nodes. You should follow the [scratch directory procedures](user-guide/quickstart.md#using-storage-with-jobs) to make sure your data is available to your cluster job.

??? question "Can I mount my own storage to the cluster?"
    No. RCC provides storage that is mounted to the cluster. This storage is specifically designed to work within the cluster network and meets performance requirements.

??? question "Where can I store reference data?"
    Reference data will be stored at `/hpc/refdata` upon request. Please see [Reference Data](storage/ref-data.md) for more information.

??? question "How do I use my group storage for collaboration outside my lab?"
    Your lab group storage can be used to collaborate with non-lab members. This is done with by-request project directories. For example, if lab `pi` would like to collaborate with some data for a project called `zephyr`, then the lab PI would contact RCC to create a project directory.

    ```bash
    $ ls -l /group/pi
    total 8
    drwxrws---. 3 root sg-pi-zephyr 512 Mar 26 13:52 zephyr
    drwxrws---. 4 root sg-pi       1024 May 19 14:04 work
    ```

    In addition to the usual `work` directory, there is now a `zephyr` project directory, which is controlled by the `sg-pi-zephyr` security group. Project data would go in that directory, and any number of collaborators (MCW researchers) can be added to the security group.

??? question "Why did my Windows SMB storage share stopped working?"
    Stale connections for Windows shares are not uncommon. If your storage share stopped working, chances are this is a stale connection. This is especially true if the storage was working one day, and you cannot connect the next day. To fix this issue, you'll need to remount the storage with the following steps.

    * Open Windows File Explorer and right-click on the drive, then select Disconnect.

    * Remount the storage using this guide <https://support.microsoft.com/en-us/windows/map-a-network-drive-in-windows-10-29ce55d1-34e3-a7e2-4801-131475f9557d>. In step #4, make sure to select `Connect using different credentials` during that process. If you are on a non-MCW managed computer, please enter your MCW username with the "MCWCORP\" domain prefix (example MCWCORP\jsmith).

## Open OnDemand

??? question "Why does the Open OnDemand web page not load?"
    Open OnDemand supports most modern browsers. However, there is no IE 11 support. To have the best experience using Open OnDemand, use the latest versions of Google Chrome, Mozilla Firefox or Microsoft Edge.

??? question "How do I use RStudio on the cluster?"
    Open OnDemand has a built-in RStudio app. You can use this to get a RStudio session on a compute node that you can access via your web browser. See the [RStudio on Open OnDemand guide](user-guide/access/ondemand.md#rstudio) for details.

??? question "How do I get a Jupyter Notebook on the cluster?"
    Open OnDemand has a built-in Jupyter Notebook app. You can use this to get a Jupyter Notebook on a compute node that you can access via your web browser. See the [Jupyter on Open OnDemand guide](user-guide/access/ondemand.md#jupyter-notebooks) for details.

??? question "Can I get a virtual desktop on the cluster?"
    Yes, [Open OnDemand](https://ondemand.rcc.mcw.edu) does have a virtual desktop app built-in. However, since the cluster compute nodes are not designed or built for desktop use, functionality may be limited.

## About RCC

??? question "What is RCC?"
    The Research Computing Center (RCC) provides infrastructure and campus-wide access to resources required for computationally intensive biomedical research. This includes shared hardware and research-specific software which is supported by MCW and research grants.

??? question "How do I acknowledge Research Computing resources in my publication?"
    Please include the following acknowledgement in any publication resulting from work on Research Computing resources:

    !!! example "Acknowledgement"
        This research was completed in part with computational resources and technical support provided by the Research Computing Center at the Medical College of Wisconsin.

??? question "Where can I find other publications from RCC users?"
    Check out the list of publications to see how Research Computing is enabling science at MCW. The list of publications is updated periodically. Please contact {{ support_email }} to add your publication.

??? question "Where can I find RCC information for my grant submission?"
    RCC provides boilerplate language for all current systems including cluster, storage, network, staffing, etc. See [Grant Assistance](grants.md) for details. Please note that RCC will also provide a letter of support upon request.

--8<-- "includes/abbreviations.md"
