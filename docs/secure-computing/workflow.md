# Running Jobs with Restricted Data

Job submission and management uses the same commands and syntax as the HPC cluster. The same partitions (queues), fairshare, and resource limits apply to jobs.  However, there are some key differences in workflow when using restricted datasets.

Here we discuss running jobs, unique workflow features for ResHPC, and references to general documentation.

## Submitting Jobs

Job submission and management uses the same commands and syntax as the HPC cluster. However, ResHPC SLURM accounts are slightly different. When submitting a ResHPC job, the SLURM account is the project ID, not the PI's NetID.

For details on submitting and managing SLURM jobs, please see [Submitting SLURM Jobs](../user-guide/jobs/running-jobs.md).

## Encrypting Restricted Data

Restricted source data must be encrypted, and remain encrypted when stored in the project's `/group` directory. Source data may only be decrypted after it is copied to the project's `/scratch` directory, and only as needed for analysis.

Please note that many restricted datasets will be delivered to you in an encrypted format, such as dbGaP data. Some genomics software can work directly with these encrypted files. However, there are a variety of encryption programs available if needed.

Please contact {{ support_email }} with questions.

## Data Staging and Workflow

Staging data for a job is very similar to using the HPC cluster. A key difference is the need to encrypt restricted data when not in use.

1. Copy encrypted data from project's `/group` source directory to `/scratch` directory.
2. Decrypt files in project's `/scratch` directory and submits jobs that use the unencrypted data.
3. Run jobs and copy results from `/scratch` directory back to project's `/group` results directory.
4. Continue with further computations using the unencrypted data in project's `/scratch` directory.
5. Finish workflow and delete files from project's `/scratch` directory.

!!! danger "Warning"
    Do not leave unencrypted data in project's `/scratch` directory. For example, if you're going on vacation, delete the unencrypted data.
