# Python

!!! abstract "Summary"
    Here we present several ways to use and customize your Python environment. As you will see, there are multiple ways to achieve the same goal. Some are more elaborate and time consuming, but also offer more choice in customization. It will be up to you to decide which solution best suits your needs. However, a few ideas here may help you along the way. We'll discuss the following methods starting with the simplest and increasing in difficulty and depth of customization.

The fastest way to start installing and using your own Python packages is to follow the [Installing Packages](#installing-packages) guide. You will be able to install, uninstall, and start using packages quickly. If you're looking for a testing environment, you should try [Python Virtual Environments](#python-virtual-environment). Virtualenvs allow you to quickly setup a reusable container that can be started, stopped, and easily deleted if needed. Best of all, you can have as many separate Python virtualenvs as you'd like. Finally, you may find that a package or script requires Python itself to be installed with some custom options. This can be done without system administrator privileges using the [custom local Python](#custom-local-python) guide. 

Python is an excellent programming and data analysis tool that is widely used in many areas of computational science. You'll find many ways to solve problems as we've shown here. If you have any questions about Python, please contact {{ support_email }}.

!!! warning "Python2 Deprecated"
    Python2 is deprecated. The developers are no longer maintaining the source code. It is recommended that all users migrate their scripts from Python2 to Python3. At the least, users should not write new scripts in for Python2.

## Using Installed Python
The HPC cluster includes Python3.9 and Intel Python3. Use `module avail python` to see available versions. Python2.7 is deprecated but installed for backwards compatibility. You should not use it for any new scripting. 

To use Python3:
```bash
$ module load python/3.9.1 
$ python
Python 3.9.1 (default, Jan 29 2021, 16:36:06)
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

To use Intel Python3:
```bash
$ module load python/intel-2021.1
Python 3.7.9 (default, Nov  4 2020, 21:30:07)
[GCC 7.3.0] :: Intel Corporation on linux
Type "help", "copyright", "credits" or "license" for more information.
Intel(R) Distribution for Python is brought to you by Intel Corporation.
Please check out: https://software.intel.com/en-us/python-distribution
>>>
```

## List Installed Packages
Many common scientific Python packages (i.e., **NumPy**, **SciPy**, **Pandas**) are already installed.

To view all installed packages:
```bash
$ module load python/3.9.1
$ pip freeze
alabaster==0.7.12
anyio==2.1.0
appdirs==1.4.4
...
```

To search for a specific package (e.g., **SciPy**>:
```bash
$ pip freeze | grep 'scipy'
scipy==1.6.0
```

## Installing Packages
Most packages can be installed in your home directory without any assistance from a system administrator. This allows you to maintain a set of local packages, built on top of the central Python installation.

To install packages to your home directory:
```bash
$ module load python/3.9.1 
$ pip install --user myPackage 
```

The `--user` flag installs packages to a default directory `~/.local/bin` in your home directory.

Then add your local Python package directory to your `$PATH` variable. Edit your `~/.bashrc` file and add the following:
```bash
PATH=~/.local/bin:$PATH
export PATH
```

Then source your updated setting:
```bash
$ source ~/.bashrc
```

## Python Virtual Environment
Users can install any Python package using `virtualenv`, which creates a virtual environment within your home directory. A Python virtual environment allows a user to locally install any package using traditional Python package installation methods. For more information on Python virtual environments, see the [Virtualenv User Guide](https://virtualenv.pypa.io/en/stable/userguide/).

### Setup
To get started with Python `virtualenv`, load a Python modulefile:
```bash
$ module load python/3.9.1 
```

Create a new virtual environment:
```bash
$ virtualenv myPython39venv
New python executable in /home/user/myPython39venv/bin/python
Installing setuptools, pip, wheel...done.
```

The `virtualenv` command automatically creates a new directory (e.g., **myPython39venv**). It then installs Python and the necessary packages **setuptools**, **pip**, and **wheel**.

To use your Python virtual environment:
```bash
$ source myPython39venv/bin/activate
(myPython39venv) $ which python
~/myPython39venv/bin/python
```

To exit your Python virtual environment:
```bash
(myPython39venv) $ deactivate 
```

### Installing Packages
Packages can be installed in a Python virtual environment using `pip` or `setup.py`.

To install a package using `pip`:
```bash
(myPython39venv) $ pip install numpy
Collecting numpy
  Downloading
Installing collected packages: numpy
Successfully installed numpy-1.19.5
(myPython39venv) $ pip freeze
numpy==1.19.5
```

The `pip` command will install packages (e.g., **NumPy** to the **site-packages** directory in your virtual environment. Installed packages can be listed with the `pip freeze` command.

Packages from the system-wide Python installations can also be included in your Python virtual environment. This must be done when creating the virtual environment.

To create a Python virtual environment with system-wide packages:
```bash
$ virtualenv --system-site-packages Python39withSysPkgs
New python executable in /home/user/Python39withSysPkgs/bin/python
Installing setuptools, pip, wheel...done.
$ source Python39withSysPkgs/bin/activate
(Python39withSysPkgs) $ pip freeze
alabaster==0.7.12
anyio==2.1.0
appdirs==1.4.4
...
```

The Python virtual environment (e.g., **Python39withSysPkgs**) now includes all packages from the system-wide Python3.6. The `pip freeze` command shows the list of packages. You could now install new packages as shown above, creating a virtual extension to the system-wide Python installations.

## Miniconda Python
Miniconda is a lightweight installation of Anaconda. It is built on the conda package manager and includes many relevant scientific computing packages. The HPC Cluster has Miniconda3 installed.

For more information, see [Conda Docs](https://conda.io/docs/index.html).

!!! warning "Installing your own Conda"
    If you install your own Conda, this will often modify your `.bashrc` file. This will cause your base Conda env to load every time you login. This is very useful if you're working on your own Linux machine. But it is very un-useful on a cluster, where we also use modulefiles to load software. In fact, your Conda installation can cause your jobs to fail in many instances. If you would like to use Conda, then please use the centrally installed Miniconda3. If you must use your own Conda, please turn off the auto activate with `conda config --set auto_activate_base false`. You can then manually activate your Conda with `source /path/to/my/conda/etc/profile.d/conda.sh && conda activate`.

### Use Miniconda Python
To load Miniconda3:
```bash
$ module load miniconda3
$ conda --version
conda 4.9.2
$ python
Python 3.8.5 (default, Sep  4 2020, 07:30:14)
[GCC 7.3.0] :: Anaconda, Inc. on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

### Virtual Environments
Create a new virtual environment (e.g., **myenv** with the `conda` command:
```bash
$ conda create -n myenv
```

To use your Miniconda virtual environment:
```bash
$ source activate myenv
```

### Installing Packages
To install packages in your conda virtual environment:
```bash
$ module load miniconda3
$ source activate myenv
$ conda install numpy
```

## Python in a Job Script
You may want to use your personal Python environment in a SLURM job on the cluster. Please review the [Writing a Job Script](../user-guide/jobs/running-jobs.md#writing-a-job-script) guide before proceeding. The following examples show various methods to use your personal Python environment.

=== "py-venv.slurm"
    ```bash
    #!/bin/bash
    #SBATCH --job-name=Python
    #SBATCH --ntasks=1
    #SBATCH --mem-per-cpu=1gb
    #SBATCH --time=00:01:00
    #SBATCH --output=%x-%j.out
    
    module load python/3.9.1
    source myPython36venv/bin/activate 
    python myPythonScript.py
    ```

=== "conda-venv.slurm"
    ```bash
    #!/bin/bash
    #SBATCH --job-name=Miniconda
    #SBATCH --ntasks=1
    #SBATCH --mem-per-cpu=1gb
    #SBATCH --time=00:01:00
    #SBATCH --output=%x-%j.out

    module load miniconda3
    source activate myenv 
    python myPythonScript.py
    ```

--8<-- "includes/abbreviations.md"