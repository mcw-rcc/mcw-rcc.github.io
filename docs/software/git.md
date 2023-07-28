# Using Git and GitHub

This is a beginner's guide to using Git and GitHub for your research. Git is a well-known version control software. GitHub is a web service used to host and manage git repositories. This may sound complicated, but this guide will cover these topics step by step, starting with version control.

## Version Control

First, what is version control and what does it have to do with git, GitHub, and my research? Version control is a method to track changes on files as you work. This is very similar to Track Changes in a Microsoft Word document. It allows you to track changes so that you can manage, and undo, edits if needed. If you've ever spent hours working on a script or document only to accidentally overwrite your changes, then you know the consequences of not using version control. Whether you're a full-stack developer or simply writing a one-off R script, version control will simplify and save your work.

## Git

Git is a version control software package. It can track changes to a file or set of files. To store the files and tracked changes, the files are grouped in a repository. Within a repository, we have the current version of all tracked files, as well as a full version history. With that information, we can use git to view old changes, compare versions, and undo unwanted edits.

## First Git Repository

To get started, we'll initialize an example git repository in a new directory.

```bash
# create a new project directory
mkdir project

# move to that project
cd project

# initialize a repository
git init
```

Create a file to track. Save the `hello-world.txt` file in the `project directory`.

=== "hello-world.txt"

```txt
Hello, World!
```

Add the file to track and commit the changes.

```bash
git add hello-world.txt
git commit -m "Initial commit"
```
