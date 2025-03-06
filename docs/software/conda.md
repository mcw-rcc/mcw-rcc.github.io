# Conda

!!! danger "Do not use Anaconda products without a license"
    [Anaconda](https://www.anaconda.com/) products, including Anaconda Distribution and Miniconda, rely on proprietary package repositories by default. Anaconda's proprietary package repositories are not free to use, even for academic research, and RCC does not centrally license them for your use. While there are free and open-source package repositories (conda-forge, bioconda, etc.), Anaconda products do not use these by default. Research Computing is using [Miniforge](https://github.com/conda-forge/miniforge){:target="_blank"} as an alternative and recommends all users do the same.

Conda is a package management system that quickly installs many useful packages and their dependencies. We use a free and open-source version called [Miniforge](https://github.com/conda-forge/miniforge){:target="_blank"}. It is mainly used for Python virtual environments, but includes many more open-source research software packages and dependencies.

## Benefits of using a Conda environment

- Creating isolated conda environments is a perfect way to manage dependencies. Some times you might need to use different programs with different dependencies that conflict with each other. With conda, you can isolate each of those programs with their corresponding dependencies.
- With conda it is very easy to keep track of what you have installed and remove, add or update any packages.
- You can create a "perfect" environment for a specific project and share it with others to make sure they are using the same version of the packages. This will also allow you to create reproducible pipelines, to ensure that you run and re-run your programs within the exact same environment.
- Conda has a large community of developers and is very easy to find help in forums whenever you run into any problems.
- It supports packages written in other languages, so, you can create dedicated environments for certain projects which depend on libraries written in other languages such as R.

## Using conda on the cluster

Research Computing provides a central installation of Miniforge. All users are encouraged to use this module to create custom environments as needed.

To load Miniforge:

```bash
module load miniforge
```

## Create an environment

To create a new environment (e.g., **myenv**):

```bash
conda create -n myenv
```

## Use an environment

To use your environment:

```bash
module load miniforge
conda activate myenv
```

## Install packages in an environment

To install packages in your conda virtual environment:

```bash
conda install numpy
```

To install a specific version of a package in your conda virtual environment:

```bash
conda activate myenv
conda install python==3.8
```

To see all the packages that you have installed in your virtual environment:

```bash
conda activate myenv
conda env list
```

To see what version of a package you have installed in your virtual environment:

```bash
conda activate myenv
conda env list | grep package_name
```

To uninstall a package within your virtual environment:

```bash
conda activate myenv
conda uninstall numpy
```

## Clone an environment

Suppose you are starting a new pipeline of a project for which you need a very similar environment to one you created before, with the exception of a few packages that use different versions. You could clone your previous environment and then make the desired changes. Make sure to deactivate the environment your are cloning, clone it and then activate your new environment.

```bash
conda deactivate myenv1
conda create -n myenv1 --clone myenv2
conda activate myenv2
```

You could also create a specification file to build an identical conda environment on the same OS either in the same or a different machine. This is perfect for sharing conda environments with collaborators. Make sure the people you share your specification file with are using your same platform. Otherwise, the packages that you installed might not be available to them or some dependencies could be missing. In this case, things might not work exactly the same as expected.

To create a specification file:

```bash
conda activate myenv
conda list --explicit > spec-file.txt
```

To use a specification file to create an identical environment on the same or a different machine:

```bash
conda create --name myenv --file spec-file.txt
conda activate myenv
```

To use a spec file to install packages into an existing environment:

```bash
conda install --name myenv 
conda activate myenv --file spec-file.txt
```

## Remove an environment

To completely remove a conda env:

```bash
conda remove --name myenv --all
```

To remove all packages within an environment:

```bash
conda remove -n myenv --all --keep-env
```

The `--keep-env` option leaves the environment intact. Use this option if you'd like to start over, but do not want to rename the environment.

## Common issues

Users should avoid mixing conda and non-conda modules in cluster jobs and workflows. For example, a pipeline requiring Python and TensorFlow should use `module load tensorflow` and not `module load tensorflow python`. The first loads the TensorFlow module which is conda based and includes Python already. The second loads the conda-based TensorFlow module and non-conda-based Python module, which may not give the desired version of Python. This simple example can be compounded with job scripts that load many modules.

Users should also avoid using the `conda init` command. If you accidentally setup conda to auto-intialize via your `.bashrc` or `.bash_profile` scripts at login, unintended issues may occur. For example, subsequent module loads may not work, or system commands may conflict with packages within your auto-init conda env. Users that have this issue will see an env name in front of their username at login, i.e. `(myenv) [netid@server ~]`. You can fix this issue with `conda config --set auto_activate_base false`.

## Additional help

The conda docs provide much more information than is practical to include here. Consult the [Managing Environments guide](https://docs.conda.io/projects/conda/en/stable/user-guide/tasks/manage-environments.html) for more information on conda envs.
