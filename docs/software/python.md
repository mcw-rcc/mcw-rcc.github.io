# Python

Python is an object-oriented programming language that is easy to write and read. It is widely used in many areas of computational science and has many useful packages built-in or available for installation.

!!! warning "Python2 Deprecated"
    Python2 is deprecated. The developers are no longer maintaining the source code. It is recommended that all users migrate their scripts from Python2 to Python3. At the least, users should not write new scripts in for Python2.

## Using Installed Python

The HPC cluster includes Python3.9 and Intel Python3. Use `module avail python` to see available versions. Python2.7 is deprecated but installed for backwards compatibility. You should not use it for any new scripting.

The first step is to load the corresponding module:

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

We recommend to install python packages in virtual environments or conda environments. Try to avoid installing with sytem Python, which will place the package files in your home directory. While this does work, it can be difficult to know what has been installed, and even more difficult to update or remove. Also, using conda will allow you to create different environments for specific software and share them with other people. For example, one program that you might need can have certain dependencies that conflict with the dependencies of another program. You can solve this by using different conda environments. For more information please visit our [Conda Guide](conda.md).

### Python Virtual Environment

A Python virtual environment is an independent set of Python packages contained in a directory. To create an environment, we use the `venv` Python module. Within an environment, we use standard package installation methods. With Python virtual environments, everything is self-contained, reproducible, and easy to manage. For more information on Python virtual environments, see the [venv guide](https://docs.python.org/3/library/venv.html){:target="_blank"}.

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
