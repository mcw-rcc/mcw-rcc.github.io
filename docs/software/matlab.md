---
version: R2021b
---
# MATLAB Parallel Server

The MATLAB Parallel Server software is an extension of the Parallel Computing Toolbox. The software runs on the HPC Cluster and can be used to run MATLAB jobs that are too large to run on your personal desktop/laptop computer. RCC has a license for 256 workers.

## Requirements

* RCC user account
* MATLAB {{ version }}
!!! warning "Desktop client"
    To use MATLAB Parallel Server, your desktop client must be licensed through MCW.

## Installation

MATLAB requires that the Parallel Server version match the client version. RCC attempts to update the MATLAB Parallel Server software whenever the central MCW license server is updated. The current version is {{ version }}. Please install the correct client version before proceeding with the Client-to-Cluster configuration.

### Configuration

1. Download the [HPC Cluster setup files](https://mcw.box.com/shared/static/piynswhbqjvonqn4k0gs56xtrn5rw6j7.zip). Unzip and save this folder to a location of your choice. Please note, this folder is necessary for the cluster setup and should be saved in a location so that it will not be erased.
2. Open outbound firewall ports '''14384-14448''' for the MATLAB program on your computer. This will require administrator privileges on your computer. Contact your department IT worker for assistance.
3. Save the following script as `startup.m` in a location on the MATLAB path that will not be deleted.
:
    === "startup.m"

    ```matlab
    % startup.m
    % Startup script identifies available interfaces, 
    % and selects correct interface.
     
    e = java.net.NetworkInterface.getNetworkInterfaces();
    while(e.hasMoreElements())
        ee = e.nextElement().getInetAddresses();
        while (ee.hasMoreElements())
            i = ee.nextElement().getHostAddress().toString();
            if contains(i,java.lang.String('141.106'))
                pctconfig('hostname',char(i));
            end
        end
    end
    ```

4. Launch the MATLAB application and navigate to **Home > Parallel > Create and Manage Clusters**.
5. Open the **Cluster Profile Manager** and select **Import**.
6. Browse to the location of the **MATLAB_R2021b_Client2Cluster** folder and select the **HPC_Cluster.mlsettings** file.
7. Select the profile in the Cluster Profile Manager and click **Edit**. Scroll down to the **Scheduler Plugin** section of the profile and change the **PluginScriptsLocation** property to point to the location of your copy of the **MATLAB_R2021b_Client2Cluster** folder. Locate the Additional Properties table. Edit the **RemoteJobStorageLocation** and **Username** properties by replacing **NetID** with your MCW username.
8. Select the **HPC Cluster** profile in the Cluster Profile Manager and select the **Validation** tab. Change the Number of workers to use to **1**. Select **Validate**. Enter your MCW password. All tests should pass.
9. Finally select the profile in the Cluster Profile Manager and select **Set as Default**.

## Upgrading

!!! tip "RCC will periodically update the MATLAB Parallel Server software to the next B version."
    To update, first desktop software to match the new cluster version. Then follow the steps above to configure the new version. Please note that your client software must be equal version or older than cluster version.}}

## Using the Cluster

There are several ways to interact with the cluster using the Parallel Computing Toolbox. The **parpool** and **batch** commands can be used to create jobs to run your code on the cluster. Examples are provided below. More information is available on the Mathworks website for [Batch Processing](https://www.mathworks.com/help/distcomp/batch.html) and [Parpool](https://www.mathworks.com/help/distcomp/parpool.html).

### Batch

The **batch** command also creates a pool of workers in a job on the cluster. It creates a remote pool of workers on the cluster that can run your script when workers are available. In comparison to the [parpool option](#parpool) option, the **batch** command does not require that you wait for a pool of workers. Instead, your script is submitted to the cluster to be run in a batch pool when workers are available.

#### Number of Workers

The **batch** command can be submitted as a pool job, where **N** is the number of workers.

```matlab
>> job = batch('mytest','Pool',N)
```
<!--A batch job requires 1 extra worker to run the pool. Therefore, the number of workers **N** is defined as **N-1**.

Examples:
 Incorrect job with 48 workers.
 >> job = batch('mytest','Pool',48)
This job will be rejected. The **batch** command adds 1 worker to run the pool and the total pool size is incorrect, 49 workers.
 Correct job with 48 workers.
 >> job = batch('mytest','Pool',47)
This job will be submitted. The **batch** command adds 1 worker to run the pool and the total pool size is correct.
 Correct job with 100 workers.
 >> job = batch('mytest','Pool',99)
This job will be submitted. The **batch** command adds 1 worker to run the pool and the total pool size is correct.
-->

#### Job Time

Each batch job is submitted to the HPC cluster with **default max walltime of 7 days** There are times (near maintenance windows) that this may not work for every job. You can add a unique time limit to your job by using **AdditionalSubmitArgs**.

```matlab
>> % Select cluster
>> c = parcluster("HPC Cluster");
>> % Set time
>> c.AdditionalProperties.AdditionalSubmitArgs = '--time=DD-HH:MM:SS';
>> % Start batch job
>> job = batch(c,"mytest");
```

A specific time limit can be added to any job. Max time is **7 days**. For example, a job with a 10 hour time limit would set **c.AdditionalProperties.AdditionalSubmitArgs = '--time=10:00:00';**.

#### Documentation

Please review the MathWorks [tutorial](https://www.mathworks.com/help/distcomp/run-a-batch-job.html) explaining batch parallel jobs:

### Parpool

The **parpool** command creates a pool of workers in a job on the cluster. It creates an interactive session using remote cluster nodes to run the pool of workers. The **parpool** command does require that enough workers are available on the cluster before the pool will start. If you do not need to run commands interactively, and/or have your code in a script, please try the [batch](#batch) example.

#### Number of Workers

The **parpool** command can be submitted with variable pool size, where **N** is the number of workers. Please try to use **parpool** sizes that are multiples of 12.

```matlab
>> parpool(N)
```

#### Job Time

Each parpool job is submitted to the HPC cluster with **default max walltime of 7 days** There are times (near maintenance windows) that this may not work for every job. You can add a unique time limit to your job by using **AdditionalSubmitArgs**.

```matlab
>> % Select cluster
>> c = parcluster("HPC Cluster");
>> % Set time
>> c.AdditionalProperties.AdditionalSubmitArgs = '--time=DD-HH:MM:SS';
>> % Open a pool of 12 workers on the cluster
>> p = c.parpool(12);
```

A specific time limit can be added to any job. Max time is **7 days** For example, a job with a 10 hour time limit would set **c.AdditionalProperties.AdditionalSubmitArgs = '--time=10:00:00';**.

#### Start Pool

To start a **parpool** on default cluster with **N** workers:

```matlab
>> parpool(N);
```

To start **parpool** on **HPC Cluster** with **N** workers:

```matlab
>> parpool('HPC Cluster',N);
```

Start a **parpool** object on a cluster with **N** workers and attach a file **myfile.m**:

```matlab
>> poolobj = parpool('HPC Cluster',N);
>> addAttachedFiles(poolobj,{'mytest.m'});
```

#### Run Code

Now that the **parpool** is started, you can run your code. The code can be run interactively or using a script.

To run code interactively:

```matlab
>> parfor i = 1:1024
>>   A(i) = sin(i*2*pi/1024);
>> end
```

Plot output:

```matlab
>> plot(A);
```

To run code with a script, create a new script:
=== "mytest.m"

```matlab
parfor i = 1:1024
  A(i) = sin(i*2*pi/1024);
end
```

Execute script:

```matlab
>> mytest
```

Plot output:

```matlab
>> plot(A)
```

#### Shutdown Pool

Please make sure to shutdown your parpool when you're done computing. To use **gcp** to shutdown your parpool:

```matlab
>> delete(gcp(poolobj))
```

## File Transfer and Management

The Matlab Parallel Computing Toolbox has multiple ways to handle file transfrer and access. For small files, the files can be auto-attached and transfer to the remote cluster at job submission. However, if your workflow requires large files, the transfer at job time becomes inconvenient. In this case, it is better to transfer files to the cluster before job submission, see [File Transfer](../storage/file-transfer.md). For more information about using data in Batch jobs, please see [Share Code with the Workers](https://www.mathworks.com/help/distcomp/share-code-with-the-workers.html).

## Cluster Usage Policy

The HPC Cluster is a shared resource. Please only use whatever resources are needed for your computation. When you're done, please make sure to stop your processes and close any batch or parpool jobs. This is especially important to ensure that fair access is maintained.

## Getting Help

Review Mathwork's [Batch Processing](https://www.mathworks.com/help/distcomp/batch-processing.html) and [Parpool](https://www.mathworks.com/help/distcomp/parpool.html) documentation.

If you have questions/concerns please contact {{ support_email }}

<!--Create a cluster object.
 c = parcluster('WilesMatlabCluster')

Create a job object associated with the cluster object.
 j = createJob(c)

Create tasks for the job. Here we create 5 tasks that each generate a 3x3 matrix of random numbers.
 createTask(j, @rand, 1, {3,3});
 createTask(j, @rand, 1, {3,3});
 createTask(j, @rand, 1, {3,3});
 createTask(j, @rand, 1, {3,3});
 createTask(j, @rand, 1, {3,3});

Submit your task to the cluster.
 submit(j)

Use the wait function to wait for the job to end or select Home > Parallel > Monitor Jobs to open the job monitor.
 wait(j)

Gather the results.
 results = fetchOutputs(j);

Print the results from each task.
 results{1:5}
-->

<!--Old setup
==Installation for Non-Interactive Use==

Using Wiles in non-interactive mode is the most efficient use of the cluster. In non-interactive use, the user can send their job from their local machine to the remote cluster for processing. The results are then copied back to the user's local machine. This requires setup of a Cluster Profile in your installation of MATLAB. Some steps are marked according to your chosen platform.

* '''Step 1:''' Obtain RCC user account with Wiles MATLAB Cluster access enabled.
----
* '''Step 2:''' Obtain MATLAB R2016a Client software. Available for purchase from the [https://infoscope.mcw.edu/RCC/Software-Catalog/Mathworks-Matlab.htm RCC Software Catalog].
---- 
* '''Step 3:''' Follow the link in your MATLAB purchase email to download the WilesCluster Profile to your desktop. 
----
* '''Step 4:''' Windows - Copy the files in:
 WilesCluster/config 
to:
 $MATLABINSTALLDIR/toolbox/local
----
* '''Step 4:''' Mac - Right click MATLAB_R2016a in the Applications directory and select Show Package Contents.

[[File:Mac_File_Install.png|500px|MATLAB install directory location.]]

Copy the files in:
 WilesCluster/config 
to:

[[File:Mac_File_Location.png|400px|MATLAB install directory location.]]
 $MATLABINSTALLDIR/toolbox/local

Copy WilesCluster Profile:
 WilesCluster/config/WilesCluster.settings
to:

[[File:Mac_Cluster_Profile.png|400px|MATLAB Cluster profile location.]]
----
* '''Step 5:''' Launch the MATLAB application, open the Parallel tab, and select Manage Cluster Profiles.
[[File:MATLAB_Menu.png|600px|MATLAB client program parallel menu.]]
----
:* '''Step 5a:''' Select Import.

[[File:MATLAB_Import.png|600px|MATLAB client program import menu.]] 
----
:* '''Step 5b:''' Windows - Add the WilesCluster.settings file found here:
 $MATLABINSTALLDIR/toolbox/local/WilesCluster.settings
----
:* '''Step 5b:''' Mac - Add the WilesCluster.settings file found here:

[[File:Mac_Cluster_Profile.png|400px|MATLAB Cluster profile location.]]
----
:* '''Step 5c:''' Click edit. In the IndependentSubmitFcn and CommunicatingSubmitFcn, replace username in with your RCC username. Click Done.

[[File:MATLAB_Username.png|600px|MATLAB client program replace username.]]
----
* '''Step 6:''' Test your connectivity to the Wiles MATLAB Cluster, click Validate, enter your Research Computing username and password (do not use identity file), the tests may take a few min. Please note that test 5 “Parallel pool test (parpool)” will fail. This is normal since “parpool” is for interactive jobs.
----
* '''Step 7:''' Set WilesCluster as your default profile:

[[File:MATLAB_Profile.png|600px|MATLAB client program select profile.]]

==Interactive Use==

Mac (requires XQuartz) and Linux users login directly to the head node with your Research Computing username and password by the following command:

 $ ssh –X username@<nowiki>wiles.rcc.mcw.ed</nowiki>u

Windows users login directly to the head node with your Research Computing username and password using X-Win32 or similar x-windows program. (Contact Research Computing for more information.)

Once logged in to the head node, run the following command:

 $ matlab

Import the cluster profile. The <nowiki>WilesCluster.se</nowiki>ttings file is in: 

 $ /usr/local/MATLAB/R2016a

Set the WilesCluster profile as default profile. To test the cluster profile, click Validate, the tests will run for a few min. All tests should complete without error.

==Example Use==
An example script for running your scripts on the Wiles Matlab Cluster is available in the WilesCluster/scripts folder. Additional information on Matlab non-interactive batch job submission is available from [http://www.mathworks.com/help/distcomp/batch.html Mathworks].-->

--8<-- "includes/abbreviations.md"
