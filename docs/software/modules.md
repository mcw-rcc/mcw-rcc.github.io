# Software Modules

We use [Lmod](https://lmod.readthedocs.io/en/latest/) to manage installed software. Each software package installed on the cluster has a corresponding modulefile, which contains information about an application's version, executable, libraries, and documentation. Using the module commands, users can add, remove, or switch versions of an application.

## Module Help
Print help information for module command:

```
$ module help
```

Print help information for a module:
```
$ module help [modulefile]
```

## Show Loaded Modules
Show currently loaded modules:
```
$ module list
```

## Show Available Modules
Show currently available modules:
```
$ module avail
```

This will output a list of modules that represent installed software.

## Show Module Information 
Print information about a module:
```
$ module show [modulefile]
```

Print module contents:
```
$ module display [modulefile]
```

## Load Module 
Add a module to the user environment:
```
$ module load [modulefile]
```

Example:
```
 $ module load gcc
```

Adds the GCC compiler to the user environment.

## Unload Module
Remove a module from the user environment:
```
$ module unload [modulefile]
```

Example:
```
$ module unload gcc
```

Removes the GCC compiler from the user environment.

## Swap Modules 
Swap a loaded module for another module:
```
$ module swap [modulefile1] [modulefile2]
```

--8<-- "includes/abbreviations.md"