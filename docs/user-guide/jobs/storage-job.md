# Staging Data

RCC storage includes a persistent <code>/group</code> space and a transient <code>/scratch</code> space. The <code>/group</code> storage is not available on compute nodes, for performance reasons. Therefore, it is necessary to stage your data into and out of <code>/scratch</code>. If your job has a large input dataset, we suggest to copy the data before the job. If input data is relatively small, you can add the staging step to the job.

!!! warning "Data staging required"
    Your job will fail and/or you will lose your job output if you do not properly stage your data to and from scratch. Contact {{ support_email }} for help with data staging.

## Workflow

1. User copies job input/supporting files from an RGS directory within `/group/{PI_NetID}/...` to their scratch directory `/scratch/u/{NetID}` or `/scratch/g/{PI_NetID}`
2. User submits job that computes with the staged job input/supporting files
3. Job finishes and user copies results from `/scratch/u/{NetID}` or `/scratch/g/{PI_NetID}`, back to `/group/{PI_NetID}/...`
4. User continues with further computations with the job input/supporting files
5. User finishes computations and deletes unneeded job input/supporting files from `/scratch/u/{NetID}` or `/scratch/g/{PI_NetID}`