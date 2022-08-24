# Singularity

## Overview

[Singularity](https://sylabs.io/guides/3.7/user-guide/) is a container solution designed for high-performance computing systems. A Singularity container is a collection of application and dependency files, which is packaged as a single portable image file. Containers are independent from the host operating system, allowing applications that are not natively supported to run on a variety of HPC resources. They are conceptually similar to Docker, but focus more on HPC. Singularity containers can be shared and distributed to support reproducible research and can be run on most HPC systems without modification as long as Singularity is installed.

## Singularity Commands

Singularity uses a series of subcommands and options controlled by a wrapper script called `singularity`. The `-h` option displays the command-line help documentation:

```bash
$ module load singularity
$ singularity -h
USAGE: singularity [global options...] <command> [command options...] ...
 
GLOBAL OPTIONS:
    -d|--debug    Print debugging information
    -h|--help     Display usage summary
    -s|--silent   Only print errors
    -q|--quiet    Suppress all normal output
       --version  Show application version
    -v|--verbose  Increase verbosity +1
    -x|--sh-debug Print shell wrapper debugging information
 
GENERAL COMMANDS:
    help       Show additional help for a command or container                  
    selftest   Run some self tests for singularity install                      
 
CONTAINER USAGE COMMANDS:
    exec       Execute a command within container                               
    run        Launch a runscript within container                              
    shell      Run a Bourne shell within container                              
    test       Launch a testscript within container                           
 
CONTAINER MANAGEMENT COMMANDS:
    apps       List available apps within a container                           
    bootstrap  *Deprecated* use build instead                                   
    build      Build a new Singularity container                                
    check      Perform container lint checks                                    
    inspect    Display container's metadata                                     
    mount      Mount a Singularity container image                              
    pull       Pull a Singularity/Docker container to $PWD                      
 
COMMAND GROUPS:
    image      Container image command group                                    
    instance   Persistent instance command group                                

 
CONTAINER USAGE OPTIONS:
    see singularity help <command>
```

For more information see the [Singularity Quick Start](https://sylabs.io/guides/3.7/user-guide/quick_start.html#interact-with-images).

### Download Pre-built Containers

Many software packages already have containers built by other users. These containers are available from external sites such as Singularity Hub or Docker Hub.

The `singularity pull` command is used to download existing containers.

#### Pull from Singularity Hub

To download a container from Singularity Hub:

```bash
$ singularity pull shub://vsoch/hello-world 
Progress |===================================| 100.0%
Done. Container is at: /home/user/vsoch-hello-world-master.sif
$ ./vsoch-hello-world-master.sif
RaawwWWWWWRRRR!!
```

You can also download and rename containers:

```bash
$ singularity pull --name myContainer.sif shub://vsoch/hello-world
Progress |===================================| 100.0%
Done. Container is at: /home/user/myContainer.sif
$ ./myContainer.sif
RaawwWWWWWRRRR!!
```

#### Pull from Docker Hub

To download a container from Docker Hub:

```bash
$ singularity pull docker://godlovedc/lolcow
WARNING: pull for Docker Hub is not guaranteed to produce the
WARNING: same image on repeated pull. Use Singularity Registry
WARNING: (shub://) to pull exactly equivalent images.
Docker image path: index.docker.io/godlovedc/lolcow:latest
Cache folder set to /home/user/.singularity/docker
[6/6] |===================================| 100.0%
Importing: base Singularity environment
Importing: /home/user/.singularity/docker/sha256:9fb6c798fa41e509b58bccc5c29654c3ff4648b608f5daa67c1aab6a7d02c118.tar.gz
...
Building Singularity image...
Singularity container built: ./lolcow.img
Cleaning up...
$ ./lolcow.img
 ________________________________________
/ It is a wise father that knows his own \
| child.                                 |
|                                        |
| -- William Shakespeare, "The Merchant  |
\ of Venice"                             /
 ----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

The pull command downloads the container from Docker, converts it to Singularity format, and builds a the container. This is different than pulling an image from Singularity Hub, where the pull command simply downloads the image. Singularity includes warnings about this fact. Since a pull from Docker Hub induces a build process, the downloaded container is not guaranteed to be the same each time. If strict reproducibility is needed, pull from Singularity Hub.

#### Build from Singularity Hub

The `singularity build` command can also be used to download existing containers.

```bash
$ singularity build hello-world.sif shub://vsoch/hello-world
Cache folder set to /home/user/.singularity/shub
Progress |===================================| 100.0%
Building from local image: /home/user/.singularity/shub/vsoch-hello-world-master.sif
Building Singularity image...
Singularity container built: hello-world.sif
Cleaning up...
$ ./hello-world.sif
RaawwWWWWWRRRR!!
```

When using the build command, you must specify a name for your container. The build command differs from pull in that it will download and then build your image using the latest Singularity image format. Additional functionality of the build command is discussed in [Build a Container](../software/singularity.md#build-a-container).

#### Build from Docker Hub

```bash
$ singularity build lolcow.sif docker://godlovedc/lolcow
Docker image path: index.docker.io/godlovedc/lolcow:latest
Cache folder set to /home/user/.singularity/docker
Importing: base Singularity environment
Importing: /home/user/.singularity/docker/sha256:9fb6c798fa41e509b58bccc5c29654c3ff4648b608f5daa67c1aab6a7d02c118.tar.gz
...
Building Singularity image...
Singularity container built: lolcow.sif
Cleaning up...
$ ./lolcow.sif
 ______________________________________ 
< Today is what happened to yesterday. >
 --------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

### Build a Container

The primary function of the build command is to create new containers either from scratch or based on existing containers. It is also used to convert between container formats. Full documentation of the build command is available at [Singularity Docs](https://sylabs.io/guides/3.7/user-guide/build_a_container.html). Here we include an example definition file and build process for a Python container.

#### Definition File

First we write a definition file for the new container. A definition file defines the container build process including software installation, runtime functionality, etc. This file can be named how you like, but it is recommended to denote definition files with the `.def` extension. We will call our definition file `Python.def`.

=== "container.def"

```bash
Bootstrap: docker
From: ubuntu:20.04

%help
# Add information describing your container.
This container includes Python.

%environment
    # Set useful environment variables here. This section is used only at runtime, not during build.
    export PATH=/opt/python/bin:$PATH
    export LD_LIBRARY_PATH=/opt/python/lib:$LD_LIBRARY_PATH

%post
    # Add build commands here. These commands are only run at build time.

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

Full documentation of definition files at [Singularity Docs](https://sylabs.io/guides/3.7/user-guide/definition_files.html).

#### Build

Build the container using the definition file:

```bash
sudo singularity build Python.sif Python.def
```

Build requires `root` or `sudo` permissions. The syntax is always `singularity build` followed by the new container name, denoted by the `.sif` extension, and the definition file.

#### Test

Copy your container file to your RCC home directory and run a test:

```bash
$ srun --ntasks=1 --mem-per-cpu=4GB --time=01:00:00 --job-name=interactive --account={PI_NetID} --pty bash
$ module load singularity
$ singularity exec Python.sif python --version
Python 2.7.15
```

Here we print the version of Python installed in the container. The exec command executes a command (i.e., `python --version`) within the container.

## Containers in Jobs

Singularity containers were designed to be run on HPC systems. RCC maintains containers for several software packages on the clusters. The following examples show an RCC built Singularity container which can be run in an interactive or batch job.

### Example Interactive job

Start an interactive job on the cluster:

```bash
srun --ntasks=1 --mem-per-cpu=4GB --time=01:00:00 --job-name=interactive --account={PI_NetID} --pty bash
```

Load the Singularity module:

```bash
module load singularity
```

Run a command in your container:

```bash
$ singularity exec Python.sif python hello_world.py
Hello, World!
```

The exec command is used to execute the `hello_world.py` script within the container.

Alternatively, shell into the container and run a command:

```bash
$ singularity shell Python.sif 
Singularity: Invoking an interactive shell within container...

Singularity Python.sif:~> python hello_world.py 
Hello, World!
```

If your container has a **%runscript** section defined, then you could also use the `singularity run` command. Choose the method that works best for you.

### Example Batch Job

=== "container.sh"

```txt
#!/bin/bash
#SBATCH --job-name=Testing
#SBATCH --ntasks=1
#SBATCH --time=00:01:00
#SBATCH --account=<PI_NetID>
#SBATCH --output=%x-%j.out

module load singularity
singularity exec Python.sif python hello_world.py
```

Submit the job:

```txt
sbatch container.sh
```

This job will execute the `hello_world.py` script within the `Python.sif` container on a compute node. The output file should contain `Hello, World!`.

## Singularity on RCC

RCC uses Singularity to install and maintain software packages that would not otherwise be available on the compute clusters. This is often due to incompatible libraries or unsupported operating system. In some cases Singularity is used to control access or functionality of an application. RCC-built containers are designed to interact with RCC clusters. They include necessary file mount points, special libraries (e.g., CUDA), and custom wrapper commands. Here we will discuss the elements needed to make your containers compatible with RCC systems.

A set of useful directories are mounted by default on RCC clusters. The following file mounts should be included for non-GPU containers:

```txt
/hpc
/scratch
```

Add to the `%post` section of your definition file:

```bash
mkdir -p /hpc /scratch
```

## Singularity and Docker

Singularity is conceptually similar to Docker and easily supports Docker images. We've shown above that Singularity can be used to download and Singularity-ize Docker images. It can also be used to build containers starting from a Docker image base (RCC does this). This allows RCC to support a wide variety of software that is already containerized with Docker. If you can find a container on Docker Hub, chances are that it will be supported through Singularity. If you already use Docker to containerize your software, then you can continue developing in Docker knowing that Singularity will easily import your images.

Full information on Singularity and Docker at [Singularity Docs](https://sylabs.io/guides/3.7/user-guide/).

## Getting Help

Contact {{ support_email }} for assistance with containers.

--8<-- "includes/abbreviations.md"
