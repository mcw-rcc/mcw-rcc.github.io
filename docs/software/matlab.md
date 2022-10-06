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
    To update, first desktop software to match the new cluster version. Then follow the steps above to configure the new version. Please note that your client software must be equal version or older than cluster version.

## Using the Cluster

There are several ways to interact with the cluster using the Parallel Computing Toolbox. The **parpool** and **batch** commands can be used to create jobs to run your code on the cluster. Examples are provided below. More information is available on the Mathworks website for [Batch Processing](https://www.mathworks.com/help/distcomp/batch.html) and [Parpool](https://www.mathworks.com/help/distcomp/parpool.html).

### Batch

The **batch** command also creates a pool of workers in a job on the cluster. It creates a remote pool of workers on the cluster that can run your script when workers are available. In comparison to the [parpool option](#parpool) option, the **batch** command does not require that you wait for a pool of workers. Instead, your script is submitted to the cluster to be run in a batch pool when workers are available.

#### Number of Workers

The **batch** command can be submitted as a pool job, where **N** is the number of workers.

```matlab
>> job = batch('mytest','Pool',N)
```

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

<!-- markdownlint-disable MD024 -->
#### Number of Workers

The **parpool** command can be submitted with variable pool size, where **N** is the number of workers. Please try to use **parpool** sizes that are multiples of 12.

```matlab
>> parpool(N)
```

#### Job Time
<!-- markdownlint-enable MD024 -->

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

--8<-- "includes/abbreviations.md"
