# Using Data in a Job

You will find three storage paths that are common on all cluster compute nodes. These include `/tmp`, `/scratch`, and `/group`. Here we'll discuss best practice use of these storage paths for different types of workflows. If you would like more detail about Research Computing storage, please see the [storage guide](../../storage/rcc-storage.md).

## Workflow

Many jobs have specific needs, but there are some best practices that apply in all cases.

- Always run a test job (preferably a small sample) to verify storage requirements
- Always check your quota to ensure sufficient storage space
- Always read/write data from fastest storage available
- Always write high I/O output to `/scratch`

For best performance, job files should be located on `/scratch`. We suggest the following workflow.

1. User copies job input/supporting files from `/group` to `/scratch`
2. User submits job that computes with the staged job input/supporting files
3. Job finishes and user copies results from `/scratch` to `/group`
4. User cleans up files in `/scratch`

--8<-- "includes/abbreviations.md"
