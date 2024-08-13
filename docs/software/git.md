# Git and GitHub

This is a beginner's guide to using git and GitHub. These are version control tools that are commonly used by software developers. We can use them in our computational research to better organize our scripts, collaborate with others, and prevent accidental loss of data. Here we will discuss step-by-step how to use git and GitHub.

!!! tip "Tutorials"
    You will need git installed if you want to follow along with the included tutorials. The HPC cluster login nodes have git installed. You can also install git on your MacOS or Windows computer.

## Version Control

Version control is a method to track changes on files as you work. The idea is similar to Microsoft Word track changes. For personal use, it allows you to manage and undo edits if needed. In a group setting, version control allows many users to contribute to the same files without conflict. If you've ever spent hours working on a script or document only to accidentally overwrite your changes, then you know the consequences of not using version control. Whether you're a full-stack developer or simply writing a one-off R script, version control will simplify and save your work.

## About git

Git is a version control software package. It can track changes to a file or set of files. To store the files and tracked changes, the files are grouped in a repository. Within a repository, we have the current version of all tracked files, as well as a full version history. With that information, we can use git to view old changes, compare versions, and undo unwanted edits.

## Configure git

Set the following options before you proceed.

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@domain"
git config --global init.defaultBranch main
```

## Create a Repository

To get started, we'll initialize an example git repository in a new directory.

```bash
# create a new directory
mkdir git-example

# move to that directory
cd git-example

# initialize a repository
git init
```

Next we create a file to track. Save the `hello-world.txt` file in the `git-example` directory.

=== "hello-world.txt"

```txt
Hello, World!
```

Use the `git status` command to view the repo status.

```bash
git status
```

The output will show untracked files. Use this same command after each of the following steps to watch the status of your changes through the process.

Now we add the file to track changes.

```bash
git add hello-world.txt
```

And commit your changes.

```bash
git commit -m "Initial commit"
```

So far we have covered the git version control software. But everything we've done is local to the computer we're working on. In many cases this will be fine. However, it is sometimes necessary or desirable to share our files with the larger community. We can do this using a remote git repository host. There are several options, but here we will discuss GitHub.

## About GitHub

[GitHub](https://github.com/){:target="_blank"} is a web service used to host and manage repositories. It offers public and private repository hosting for free. Another popular and free solution is [GitLab](https://about.gitlab.com/){:target="_blank"}.

!!! tip " GitHub account"
    You will need a free GitHub account for the tutorial below.

## Host a repository on GitHub

To get started, [create a new remote repository](https://github.com/new){:target="_blank"}. Fill in the repository name, select public or private, and create repository.

Now we connect your local repository to the remote GitHub repository and push our changes.

```bash
git remote add origin https://github.com/your_user_name/git-example.git
git branch -M main
git push -u origin main
```

You can also clone (i.e. download) a remote repository from GitHub to your local computer. This is useful if you created a remote repository first, or when using code from another entity.

To clone a repository, locate the address. You can always find this using the green `<> Code` button on the repository's webpage. Use the **HTTPS** URL.

```bash
git clone https://github.com/octocat/Hello-World.git
```

## Security considerations

You should not store sensitive information in any remote repository, including GitHub, GitLab, etc. In fact, you should avoid sensitive information in your research files at all times. This includes PHI, passwords, etc. Remember, most of the data you upload to GitHub will be publically accessible. It is very easy to accidentally leave a password or sensitive file in your code, which you then upload.

Also consider the code you might clone from other users on GitHub. Use caution to avoid downloading malicious software.

## Additional Info

Git and GitHub are very well-documented and popular tools. There are many great tutorials to make using git easier. We encourage you to seek out more tutorials as needed.

There are also many software tools to help work with Git and GitHub. These include [GitHub Desktop](https://desktop.github.com/){:target="_blank"} and [VS Code](https://code.visualstudio.com/){:target="_blank"}, among many others.