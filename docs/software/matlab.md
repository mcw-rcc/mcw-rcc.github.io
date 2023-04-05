---
version: R2022b
---
# MATLAB Parallel Server

The MATLAB Parallel Server software is an extension of the Parallel Computing Toolbox. The software runs on the HPC Cluster and can be used to run MATLAB jobs that are too large to run on your personal desktop/laptop computer. RCC has a license for 256 workers.

## Requirements

- RCC user account
- MATLAB {{ version }} (purchased from MCW)

## Setup

!!! warning "Version requirement"
    MATLAB requires that the Parallel Server version match the client version. Please make sure your client MATLAB version is {{ version }}.

### Download Plugin Files

Download the [scheduler plugin files](https://mcw.box.com/shared/static/u3pyc0elqy9dn5movaa6sqv4ypc273kp.zip) that help connect your MATLAB client to the HPC cluster. Unzip and save this folder to a location of your choice. Please note, this folder should be saved in a location that will not be moved or erased.

### Add Startup Script

Save the following script as `startup.m` in a location on the MATLAB path that will not be deleted.

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

### Add Cluster Profile

1. Launch the MATLAB application and select **Home > Parallel > Create and Manage Clusters** to open the **Cluster Profile Manager** window. Select **Import**, browse to the location of the **MATLAB_{{ version }}_Client2Cluster** folder, and select the **HPC_Cluster.mlsettings** file.
2. Locate **HPC Cluster** profile in the Cluster Profile Manager and select **Edit**.
    - Locate the **Scheduler Plugin** section of the profile. Set the **PluginScriptsLocation** property to location of the **MATLAB_{{ version }}_Client2Cluster** folder.
    - Locate the **Additional Properties** table. Set the **RemoteJobStorageLocation** property to `/scratch/g/PI_NetID`, where `PI_NetID` is your PI's username. Set the **Username** property to your MCW username.  
3. Select **Done** editing and set the new profile as default.

### Validation

Select the **Validation** tab. Change the **Number of workers to use** to **1**. Select **Validate** and enter your MCW password when prompted. All tests should pass.

## Upgrading

RCC will periodically update the MATLAB Parallel Server software to the next B version. After you upgrade your client to match, follow the steps above to configure the new version.

## Using the Cluster

There are several ways to interact with the cluster using the Parallel Computing Toolbox. The **parpool** and **batch** commands can be used to create jobs to run your code on the cluster. Examples are provided below. More information is available on the Mathworks website for [Batch Processing](https://www.mathworks.com/help/distcomp/batch.html) and [Parpool](https://www.mathworks.com/help/distcomp/parpool.html).

### Batch

The **batch** command also creates a pool of workers in a job on the cluster. It creates a remote pool of workers on the cluster that can run your script when workers are available. In comparison to the [parpool option](#parpool) option, the **batch** command does not require that you wait for a pool of workers. Instead, your script is submitted to the cluster to be run in a batch pool when workers are available.

The **batch** command can be submitted as a pool job, where **N** is the number of workers.

```matlab
>> job = batch('mytest','Pool',N)
```

Your batch jobs are submitted to the HPC cluster with **default max walltime of 8 hours**. There are times (near maintenance windows) that this may not work for every job. You can add a unique time limit to your job by using **AdditionalSubmitArgs**.

```matlab
>> % Select cluster
>> c = parcluster("HPC Cluster");
>> % Set time
>> c.AdditionalProperties.AdditionalSubmitArgs = '--time=DD-HH:MM:SS';
>> % Start batch job
>> job = batch(c,"mytest");
```

A specific time limit can be added to any job. Max time is **7 days**. For example, a job with a 10 hour time limit would set **c.AdditionalProperties.AdditionalSubmitArgs = '--time=10:00:00';**.

!!! info "Additional Information"
    Please review the MathWorks [tutorial](https://www.mathworks.com/help/distcomp/run-a-batch-job.html) explaining batch parallel jobs.

### Parpool

The **parpool** command creates a pool of workers in a job on the cluster. It creates an interactive session using remote cluster nodes to run the pool of workers. The **parpool** command does require that enough workers are available on the cluster before the pool will start. If you do not need to run commands interactively, and/or have your code in a script, please try the [batch](#batch) example.

The **parpool** command can be submitted with variable pool size, where **N** is the number of workers. Please try to use **parpool** sizes that are multiples of 12.

```matlab
>> parpool(N)
```

Each parpool job is submitted to the HPC cluster with **default max walltime of 7 days** There are times (near maintenance windows) that this may not work for every job. You can add a unique time limit to your job by using **AdditionalSubmitArgs**.

```matlab
>> % Select cluster
>> c = parcluster("HPC Cluster");
>> % Set time
>> c.AdditionalProperties.AdditionalSubmitArgs = '--time=DD-HH:MM:SS';
>> % Open a pool of 12 workers on the cluster
>> p = c.parpool(12);
```

A specific time limit can be added to any job. Max time is **7 days** For example, a job with a 10 hour time limit would set `c.AdditionalProperties.AdditionalSubmitArgs = '--time=10:00:00';`.

To start **parpool** on **HPC Cluster** with **N** workers:

```matlab
>> parpool('HPC Cluster',N);
```

Start a **parpool** object on a cluster with **N** workers and attach a file `myfile.m`:

```matlab
>> poolobj = parpool('HPC Cluster',N);
>> addAttachedFiles(poolobj,{'mytest.m'});
```

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

Please make sure to shutdown your parpool when you're done:

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
