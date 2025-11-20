# Containers

## Overview

[Apptainer](https://apptainer.org/docs/user/1.3/){:target="_blank"} (formerly Singularity) is a container solution designed for high-performance computing systems. A container is a collection of application and dependency files, which is packaged as a single portable image file. Containers are independent from the host operating system, allowing applications that are not natively supported to run on a variety of HPC resources. They are conceptually similar to Docker, but focus more on HPC. Apptainer containers can be shared and distributed to support reproducible research and can be run on most HPC systems without modification as long as Apptainer is installed.

## Apptainer Commands

Apptainer uses a series of subcommands and options controlled by a wrapper script called `apptainer`. The `-h` option displays the command-line help documentation:

```txt
$ module load apptainer
$ apptainer -h

Linux container platform optimized for High Performance Computing (HPC) and
Enterprise Performance Computing (EPC)

Usage:
  apptainer [global options...]

Description:
  Apptainer containers provide an application virtualization layer enabling
  mobility of compute via both application and environment portability. With
  Apptainer one is capable of building a root file system that runs on any
  other Linux system where Apptainer is installed.

Options:
      --build-config    use configuration needed for building containers
  -c, --config string   specify a configuration file (for root or
                        unprivileged installation only) (default
                        "/hpc/apps/apptainer/1.3.0/etc/apptainer/apptainer.conf")
  -d, --debug           print debugging information (highest verbosity)
  -h, --help            help for apptainer
      --nocolor         print without color output (default False)
  -q, --quiet           suppress normal output
  -s, --silent          only print errors
  -v, --verbose         print additional information
      --version         version for apptainer

Available Commands:
  build       Build an Apptainer image
  cache       Manage the local cache
  capability  Manage Linux capabilities for users and groups
  checkpoint  Manage container checkpoint state (experimental)
  completion  Generate the autocompletion script for the specified shell
  config      Manage various apptainer configuration (root user only)
  delete      Deletes requested image from the library
  exec        Run a command within a container
  help        Help about any command
  inspect     Show metadata for an image
  instance    Manage containers running as services
  key         Manage OpenPGP keys
  keyserver   Manage apptainer keyservers
  oci         Manage OCI containers
  overlay     Manage an EXT3 writable overlay image
  plugin      Manage Apptainer plugins
  pull        Pull an image from a URI
  push        Upload image to the provided URI
  registry    Manage authentication to OCI/Docker registries
  remote      Manage apptainer remote endpoints
  run         Run the user-defined default command within a container
  run-help    Show the user-defined help for an image
  search      Search a Container Library for images
  shell       Run a shell within a container
  sif         Manipulate Singularity Image Format (SIF) images
  sign        Add digital signature(s) to an image
  test        Run the user-defined tests within a container
  verify      Verify digital signature(s) within an image
  version     Show the version for Apptainer

Examples:
  $ apptainer help <command> [<subcommand>]
  $ apptainer help build
  $ apptainer help instance start


For additional help or support, please visit https://apptainer.org/help/
```

For more information see the [Apptainer Quick Start guide](https://apptainer.org/docs/user/1.3/quick_start.html#overview-of-the-apptainer-interface){:target="_blank"}.

### Download Pre-built Containers

Many software packages already have containers built by developers that you can download with the `pull` or `build` commands. These containers are available from external sites such as the [Container Library](https://cloud.sylabs.io/library){:target="_blank"} or [Docker Hub](https://hub.docker.com/){:target="_blank"}.

The `apptainer pull` command will download a OCI image from a remote repository, combining the layers to a usable SIF image.

```txt
$ apptainer pull docker://ghcr.io/apptainer/lolcow
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
INFO:    Creating SIF file...
$ ./lolcow_latest.sif
 ______________________________
<                              >
 ------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

The `build` command has many more uses for building containers from scratch or converting formats, which we cover later. For the purpose of downloading a pre-built container, `build` differs from `pull` by converting the image to the latest Apptainer image format after downloading it. Further, `build` allows for custom naming of the downloaded container.

```txt
$ apptainer build my_lolcow.sif docker://ghcr.io/apptainer/lolcow
INFO:    Starting build...
INFO:    Creating SIF file...
INFO:    Build complete: my_lolcow.sif
$ ./my_lolcow.sif
 ______________________________
<                              >
 ------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

Because these containers are already built, they are immutable. So, you can run things but not install things inside them. To do this, you would build a container, which is explained in the next section.

### Build a Container

The `build` command is used to create new containers either from scratch or based on existing containers. It can also used to convert between container formats. As an example, we'll discuss the process of building custom Python container from scratch.

Please see the [Apptainer docs](https://apptainer.org/docs/user/1.3/build_a_container.html){:target="_blank"} for more info.

#### Definition File

First we write a definition file for the new container. A definition file defines the container build process including software installation, runtime functionality, etc. This file can be named how you like, but it is recommended to denote definition files with the `.def` extension. We will call our definition file `py_container.def`. See below for an explanation of each section.

=== "py_container.def"

```bash
Bootstrap: docker
From: ubuntu:20.04

%help
  This container includes Python.

%environment
    export PATH=/opt/python/bin:$PATH
    export LD_LIBRARY_PATH=/opt/python/lib:$LD_LIBRARY_PATH

%post
    # Create useful bind points for needed file systems. The following are set by default in RCC and should always be included for containers used in RCC.
    mkdir -p /scratch /hpc

    # Install necessary packages.
    apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        gcc-multilib \
        ca-certificates \
        zlib1g-dev \
        libssl-dev \
        curl
    apt-get clean
    rm -rf /var/lib/apt/lists/*

    # Install Python from source.
    export PATH=/opt/python/bin:$PATH
    curl https://www.python.org/ftp/python/2.7.15/Python-2.7.15.tgz -o Python-2.7.15.tgz
    tar -xvf Python-2.7.15.tgz && cd Python-2.7.15
    ./configure --prefix=/opt/python && make && make install
    rm -rf Python-2.7.15*

    # Install pip package manager.
    curl https://bootstrap.pypa.io/get-pip.py | python

    # Install some useful Python packages.
    pip install --upgrade numpy scipy matplotlib 

%test
    /opt/python/bin/python --version
```

As you can see in the previous example, a definition file has two parts. First is the header, which contains information about the base operating system for the container. `Bootstrap` is the first keyword and it's mandatory. It will specify the bootstrap agent. The most common ones are: `library`, `docker`, `shub`, or `localimage`. When using `library` as bootstrap agent, the next keyword must be `From`, followed by the operating system and the version. For example, `From: ubuntu:22.04`.

The second part of a definition file is the sections, which execute commands at different times while the container is built or run. All sections are optional. The following table explains them in detail:

| Section | Explanation |
| --- | --- |
| `%files` | Use this section to copy files from the host into the container. Never copy files into /home, /tmp, or any other directories that are bound at runtime. |
| `%environment` | Specify variables that will be set **at run** time in this section (NOT available when building the container). Variables set here are global (available to all apps). |
| `%post` | In this section you can: specify variables that will be set **at build time**, download files from the internet with tools like git and wget, install new software and libraries, write configuration files, create new directories. Never install things into /home, /tmp, or any other directories that are bound at runtime. |
| `%runscript` | This section is executed when the container is run via the `apptainer run` command or by executing the container directly as a command. Any arguments passed to the container will be processed in this section. We can use here variables set in`%environment` or `%post`. |
| `%test` | This section runs at the end of the build process to validate the container using a method of your choice. |
| `%labels` | You can add metadata in this section with name-value format such as `Author Me` or `Version v1.0`. |
| `%help` | Use this section to explain to the user how to interact with the container. |

Full documentation of definition files at [Apptainer Docs](https://apptainer.org/docs/user/1.3/definition_files.html){:target="_blank"}.

Examples of definition files: [Apptainer examples](https://github.com/apptainer/apptainer/tree/main/examples){:target="_blank"}.

#### Build

Build the container using the definition file:

```bash
sudo apptainer build py_container.sif py_container.def
```

The syntax is always `apptainer build` followed by the new container name, denoted by the `.sif` extension, and the corresponding definition file.

!!! info "root or sudo required"
    Building from a definition file requires `root` or `sudo` permissions. This is not possible on the HPC cluster. Instead we recommend to use a virtual machine running on your desktop or send a container build request to {{ support_email }}.

#### Test

Copy your container file to your RCC home directory and run a test:

```bash
$ srun --ntasks=1 --mem-per-cpu=4GB --time=01:00:00 --job-name=interactive --account={PI_NetID} --pty bash
$ module load apptainer
$ apptainer exec py_container.sif python --version
Python 2.7.15
```

Here we print the version of Python installed in the container. The exec command executes a command (i.e., `python --version`) within the container.

## Containers in Jobs

Apptainer containers were designed to be run on HPC systems. RCC maintains containers for several software packages on the clusters. The following examples show an RCC built container which can be run in an interactive or batch job.

### Example Interactive job

Start an interactive job on the cluster:

```bash
srun --ntasks=1 --mem-per-cpu=4GB --time=01:00:00 --job-name=interactive --account={PI_NetID} --pty bash
```

Load the Apptainer module:

```bash
module load apptainer
```

Run a command in your container:

```bash
$ apptainer exec  py_container.sif python hello_world.py
Hello, World!
```

The exec command is used to execute the `hello_world.py` script within the container.

Alternatively, shell into the container and run a command:

```bash
$ apptainer shell  py_container.sif 
Apptainer> python hello_world.py 
Hello, World!
```

If your container has a **%runscript** section defined, then you could also use the `apptainer run` command. Choose the method that works best for you.

### Example Batch Job

=== "container.sh"

```txt
#!/bin/bash
#SBATCH --job-name=Testing
#SBATCH --ntasks=1
#SBATCH --time=00:01:00
#SBATCH --account=<PI_NetID>
#SBATCH --output=%x-%j.out

module load apptainer
apptainer exec py_container.sif python hello_world.py
```

Submit the job:

```txt
sbatch container.sh
```

This job will execute the `hello_world.py` script within the `py_container.sif` container on a compute node. The output file should contain `Hello, World!`.

## Apptainer on RCC

RCC uses Apptainer to install and maintain software packages that would not otherwise be available on the compute clusters. This is often due to incompatible libraries or unsupported operating system. In some cases Apptainer is used to control access or functionality of an application. RCC-built containers are designed to interact with RCC clusters. They include necessary file mount points, special libraries (e.g., CUDA), and custom wrapper commands. Here we will discuss the elements needed to make your containers compatible with RCC systems.

A set of useful directories are mounted by default on RCC clusters. The following file mounts should be included for non-GPU containers:

```txt
/hpc
/scratch
```

Add to the `%post` section of your definition file:

```bash
mkdir -p /hpc /scratch
```

## Apptainer and Docker

Apptainer is conceptually similar to Docker and easily supports Docker images. We've shown above that Apptainer can be used to download and Apptainer-ize Docker images. It can also be used to build containers starting from a Docker image base (RCC does this). This allows RCC to support a wide variety of software that is already containerized with Docker. If you can find a container on Docker Hub, chances are that it will be supported through Apptainer. If you already use Docker to containerize your software, then you can continue developing in Docker knowing that Apptainer will easily import your images.

Full information on Apptainer and Docker at [Apptainer Docs](https://apptainer.org/docs/user/1.3/docker_and_oci.html){:target="_blank"}.

## Getting Help

Contact {{ support_email }} for assistance with containers.

--8<-- "includes/abbreviations.md"
