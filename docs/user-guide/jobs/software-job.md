# Loading Software in a Job

For most jobs, you'll need to load software prior to running your commands. Module commands are used to add a software environment, including executable commands and environment variables. For convenience, you can use an abbreviation `ml`, in place of the `module` command.

```bash
#!/bin/bash
#SBATCH ...
#SBATCH ...
#SBATCH ...

ml reset
ml load R/4.1.1

Rscript ...
```

Please see the [software modules guide](../../software/modules.md){:target="_blank"} for more detail.
