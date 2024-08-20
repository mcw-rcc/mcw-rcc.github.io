# Conda Virtual Environment

Conda is a package management system that quickly installs many useful packages and their dependencies. We use a lightweight version called Miniconda. It is mainly used for Python virtual environments, but includes many more packages including open-source research software and dependencies.

## Benefits of using a Conda environment

- Creating isolated conda environments is a perfect way to manage dependencies. Some times you might need to use different programs with different dependencies that conflict with each other. With conda, you can isolate each of those programs with their corresponsing dependencies.
- With conda it is very easy to keep track of what you have installed and remove, add or update any packages.
- You can create a "perfect" environmnent for a specific project and share it with others to make sure they are using the same version of the packages. This will also allow you to create reproducible pipelines, to ensure that you run and re-run your programs within the exact same environment.
- Conda has a large community of developers and is very easy to find help in forums whenever you run into any problems.
- It supports packages written in other languages, so, you can create dedicated environments for certain projects which depend on libraries written in other languages such as R.

For more information, see [Conda Docs](https://conda.io/projects/conda/en/stable/index.html){:target="_blank"}.

!!! warning "Installing your own Conda"
    If you install your own Conda, this will often modify your `.bashrc` file. This will cause your base Conda env to load every time you login. This is very useful if you're working on your own Linux machine. But it is very un-useful on a cluster, where we also use modulefiles to load software. In fact, your Conda installation can cause your jobs to fail in many instances. If you would like to use Conda, then please use the centrally installed Miniconda3. If you must use your own Conda, please turn off the auto activate with `conda config --set auto_activate_base false`. You can then manually activate your Conda with `source /path/to/my/conda/etc/profile.d/conda.sh && conda activate`.

To get started, load miniconda3:

```bash
module load miniconda3
```

To create a new virtual environment (e.g., **myenv**) with the `conda` command:

```bash
conda create -n myenv
```

To use your Miniconda virtual environment:

```bash
conda activate myenv
```

To install packages in your conda virtual environment:

```bash
conda activate myenv
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
conda unisinstall numpy
```

To clone an existing conda environment:
Suppose you are starting a new pipeline of a project for which you need a very similar environment to one you created before, with the exeption of a few packages that use different versions. You could clone your previous environment and then make the desired changes. Make sure to deactivate the environment your are clonning, clone it and then activate your new environment.

```bash
conda deactivate myenv1
conda create -n myenv1 --clone myenv2
conda activate myenv2
```

You could also create a specification file to build an identical conda environment on the same OS either in the same or a different machine. This is perfect for sharing conda environments with labmates or collaborators. Make sure the people you sare your specification file with are using your same platform. Otherwise, the packages that you installed might not be available to them or some dependencies could be missing. In this case, things might not work exactly the same as expected.

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
