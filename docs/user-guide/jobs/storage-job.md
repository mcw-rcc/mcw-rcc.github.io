# Using Data in a Job

You will find three storage paths that are common on all cluster compute nodes. These include `/tmp`, `/scratch`, and `/group`. Here we'll discuss best practice use of these storage paths for different types of workflows. If you would like more detail about Research Computing storage, please see the [storage guide](../../storage/rcc-storage.md).

## Workflows

Many workflows have specific needs (covered below), but there are some best practices that apply in all cases.

- Always run a test job (preferably a small sample) to verify storage requirements
- Always check your quota to ensure sufficient storage space
- Always read/write data from fastest storage available
- Always write high I/O output to `/scratch`

!!! warning "Avoid high I/O on `/group`"

    High input/output processes are those that read from, and write to the file system very often. This can cause issues with the `/group` file system, which is not designed for high I/O. These issues might include slowing down the file system for all users, or worse. RCC admins will kill any job or process that causes high I/O on the `/group` file system.

### General

For best performance, files can be read/written with `/scratch`. To enable this workflow, you'll need to stage your data into and out of `/scratch`.

1. User copies job input/supporting files from `/group` to `/scratch` either before or during the job
2. User submits job that computes with the staged job input/supporting files
3. Job finishes and user copies results from `/scratch` to `/group` either during or after the job
4. User cleans up files in `/scratch`

!!! info "Performance consideration"

    While best performance is good, some general workflows may not need to use `/scratch`. In this case you can leave input data in `/group` and write the output to `/scratch`. This will save time by not having to copy data, but may lose some performance.

### Large datasets

Many groups have large datasets that may not fit within the `/scratch` quota. In this case it is best practice to leave your input dataset in `/group`. Your jobs should read input files from `/group`, and write output to `/scratch`. If your output files are also large, and low I/O, they can be written to `/group` as well.

### High I/O

Workflows that read or write files often (i.e., high I/O), must use `/scratch` for both input and output. This means that you will have to stage your data into and out of `/scratch`. If your job has a large input dataset, we suggest to stage the data before the job. If input data is relatively small, you can add the staging step to the job.

### Unknown workflow needs

In some cases, a workflow may be both large and high I/O. In this case you should run tests (benchmarks) to find the best workflow.

## Example job scripts
<!-- markdownlint-disable MD046 -->
=== "stage-data.slurm"

    ```bash
    #!/bin/bash
    #SBATCH --job-name=large-data
    ....
    
    # copy input files from /group to /scratch
    # if files are too large, you can skip copy to /scratch
    cp -R /group/pi_netid/work/path/to/files /scratch/g/pi_netid/user/path

    # do some computational work with output going to /scratch
    python3 test.py -i input -o /scratch/g/pi_netid/user/path/output

    # copy results back to /group
    cp -R /scratch/g/pi_netid/user/path/output /group/pi_netid/work/path/to/files
    ```
<!-- markdownlint-enable MD046 -->

--8<-- "includes/abbreviations.md"
