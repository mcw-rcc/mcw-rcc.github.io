# Python

Python is an object-oriented programming language that is easy to write and read. It is widely used in many areas of computational science and has many useful packages built-in or available for installation.

!!! warning "Python2 Deprecated"
    Python2 is deprecated. The developers are no longer maintaining the source code. It is recommended that all users migrate their scripts from Python2 to Python3. At the least, users should not write new scripts in for Python2.

## Using Installed Python

The HPC cluster includes Python3.9 and Intel Python3. Use `module avail python` to see available versions. Python2.7 is deprecated but installed for backwards compatibility. You should not use it for any new scripting.

To use Python3:

```bash
module load python/3.9.1
```

To use Intel Python3:

```bash
module load python/intel-2021.1
```

## List Installed Packages

Many common scientific Python packages (i.e., **NumPy**, **SciPy**, **Pandas**) are already installed.

To view all installed packages:

```bash
pip freeze
```

To search for a specific package (e.g., **SciPy**):

```bash
pip freeze | grep 'scipy'
```

## Installing Packages

We recommend to install python packages in virtual environments or conda environments. Try to avoid installing with sytem Python, will place the package files in your home directory. While this does work, it can be difficult to know what has been installed, and even more difficult to update or remove.

### Conda Virtual Environment

Conda is a package management system that quickly installs many useful packages and their dependencies. We use a lightweight version called Miniconda. It is mainly used for Python virtual environments, but includes many more packages including open-source research software and dependencies.

For more information, see [Conda Docs](https://conda.io/projects/conda/en/stable/index.html).

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

### Python Virtual Environment

A Python virtual environment is an independent set of Python packages contained in a directory. To create an environment, we use the `venv` Python module. Within an environment, we use standard package installation methods. With Python virtual environments, everything is self-contained, reproducible, and easy to manage. For more information on Python virtual environments, see the [venv guide](https://docs.python.org/3/library/venv.html).

To get started, load Python:

```bash
module load python/3.9.1 
```

To create a new virtual environment:

```bash
python -m venv pythonEnv
```

To use your virtual environment:

```bash
source pythonEnv/bin/activate
```

To exit your Python virtual environment:

```bash
deactivate 
```

To install a package using `pip`:

```bash
pip install numpy
```

The `pip` command will install packages to the **site-packages** directory in your virtual environment. Installed packages can be listed with the `pip freeze` command.

Packages from the system-wide Python installations can also be included in your Python virtual environment. This must be done when creating the virtual environment.

To create and use a Python virtual environment with system-wide packages:

```bash
python -m venv --system-site-packages pythonEnvSysPkgs
source pythonEnvSysPkgs/bin/activate
```

The Python virtual environment (e.g., **pythonEnvSysPkgs**) now includes all packages from the system-wide `python/3.9.1`. The `pip freeze` command shows the list of packages. You could now install new packages as shown above, creating a virtual extension to the system-wide Python installations.

## Python in a Job Script

You may want to use your personal Python environment in a SLURM job on the cluster. Please review the [Writing a Job Script](../user-guide/jobs/running-jobs.md#writing-a-job-script) guide before proceeding. The following examples show various methods to use your personal Python environment.

<!-- markdownlint-disable MD046 -->
=== "py-venv.slurm"
    ```txt
    #!/bin/bash
    #SBATCH --job-name=Python
    #SBATCH --ntasks=1
    #SBATCH --mem-per-cpu=1gb
    #SBATCH --time=00:01:00
    #SBATCH --output=%x-%j.out

    module load python/3.9.1
    source pythonEnv/bin/activate
    python script.py
    ```

=== "conda-venv.slurm"
    ```txt
    #!/bin/bash
    #SBATCH --job-name=Miniconda
    #SBATCH --ntasks=1
    #SBATCH --mem-per-cpu=1gb
    #SBATCH --time=00:01:00
    #SBATCH --output=%x-%j.out

    module load miniconda3
    conda activate myenv
    python myPythonScript.py
    ```
<!-- markdownlint-enable MD046 -->

--8<-- "includes/abbreviations.md"
