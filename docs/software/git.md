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

## Using Git Branches

Git branches allow you to create isolated spaces to develop new features, modify code or fix bugs without affecting your main line of development. When the code in the new branch is stable and finished, you can merge it with the main branch. Each repository can have multiple branches and you can switch between them very easily. Remember that the `git` commands that you execute will modify the repository in which you are located. To change repository, navigate to the folder of the corresponding repository.

### Download the latest changes from the remote repository

Before creating a new branch, it is good idea to download the latest changes from the remote repository in order to avoid potential merge conflicts. You can first download the list of commits and changes that have been done in the remote repository using `git fetch`. This will not integrate those changes with your local repository, and will not modify your local files, it will just give you a view of what has happened in your remote repository.

```bash
# Download the changes in the remote repository
git fetch origin
# Visualize the status of the local repository compared to the remote
git status
```

If your main branch is up to date with `origin/master` (the remote repository in GitHub), you can safely create a new branch and proceed with the instructions on how to create a new branch. If your local repository is a few commits ahead and no commits behind, you can also continue working on it and merge all your changes when ready. However, if you are a few commits behind the remote branch, you can try to `pull` all changes from the remote repository in order to update the local. If there are no conflicts, your branch will be merged with the remote (locally) and you can now safely create a new branch and do any necessary edits to your code:

```bash
git pull origin
```

If there are any conflicts, you must resolve them manually before merging. Please see the section below on how to resolve conflicts.

### Creating a new branch

```bash
# Create a new branch
$ git branch test
# See the list of branches in the current repository
# The current branch has an asterisk
$ git branch
* main
test
# Switch to my new branch
# Any edits that I do after this, will be created on the new branch
$ git checkout test
Switched to branch 'test'
$ git branch
  main
* test
```

You can create a new branch and switch to it in one command:

```bash
git checkout -b test2
```

### Updating your remote repository with the new branch

After editing your files, you can use `git status` to see the list of files that have been deleted, created or modified but not yet committed. At this point the changes are local and have not been updated in your remote repository. You might see a list of untracked files, these are those that are new (since the latest commit) and haven't been added yet to your remote repository. In order to commit any changes, you first must add them to the Staging Environment and then commit everything that is in this environment. The Staging Environment tells Git what you want to commit:

```bash
# Add all modified files to the Staging Environment
$ git add --all
# Add a specific file to the Stating Environment
$ git add myfile.txt
# Add a specific directory to the Stating Environment
$ git add mydirectory/
# Add a list of files to the Staging Environment
$ git add myfile1.txt myfile2.png myfile2.csv
# To remove a file from the Staging Environment
$ git reset HEAD myfile.txt
# Commit changes to your new branch
# Add a short message to explain the changes made
$ git commit -m "Added new functionality to the brain extraction script"
# Finally, you need to push your commit from the local repository to the remote one in GitHub
# This will synchronize both repositories
# origin is the name of your remote connection that points to the clone repository (see the remote add command in the previous section)
$ git push origin test
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 486 bytes | 486.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
remote:
remote: Create a pull request for 'updates' on GitHub by visiting:
remote:      https://github.com/account/repository/pull/new/test
remote:
To https://github.com/account/repository.git
 * [new branch]      test -> updates
```

If you get an authentication error, make sure that your [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens){:target="_blank"} is not expired. After successfully running the `push` command, your new branch is updates in the remote repository. But remember that it is not yet merged with the main code. Once you are done with any edits, you can merge the branch.

### Merging my local branch with the main remote branch

If the repository belongs to you and you don't need to have anybody review the changes you have made, you are now ready to merge your local and remote branches and publish your updates.

```bash
# Switch to the main branch
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
# Merge the current branch (main) with test
$ git merge test
# Master and test are merged, we can delete test
$ git branch -d test
Deleted branch test (was 5f4472c).
# If you haven't close the command line and decide you didn't want to delete the branch for some reason, you can restore it
$ git branch test 5f4472c
# Check the status of the local repository compared to the remote
$ git status
# Push the commit to synchronize the repositories
$ git push origin
```

On the other hand, if the repository doesn't belong to you or you have collaborators and you need them to check the changes you made before merging with the main branch, you should create a pull request. You can do this by following the link printed when you executed the `git push origin test` command. In this example, that would be `https://github.com/account/repository/pull/new/test`.

### Resolving merge conflicts

You must resolve all conflicts before you can successfully merge your branch with the base branch.

1. Follow the steps in the [GitHub Docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-using-the-command-line){:target="_blank"} page to solve simple merge conflicts in specific files.
2. If the merge conflict arose because the main branch progressed since you started working on the branch that you want to merge, you will be some commits behind and some commits ahead of the main branch. In this case you will want to obtain the latest changes done to the main branch while keeping the ones done on your current branch. To accomplish this, you will need to [rebase your branch](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase#:~:text=Most%20developers%20like%20to%20use,the%20%E2%80%9Cofficial%E2%80%9D%20project%20history.){:target="_blank"}. You can find in [Github Docs](https://docs.github.com/en/get-started/using-git/about-git-rebase) {:target="_blank"} additional information on rebasing your branch.
3. To investigate further more complicated conflicts, you can [check the list of commits in the repository's history](https://www.freecodecamp.org/news/git-log-command/){:target="_blank"}.
4. You might also need to [undo changes](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting#:~:text=For%20this%20reason%2C%20git%20revert,is%20for%20undoing%20uncommitted%20changes.){:target="_blank"} before re-trying to merge, such as reverting some commits.
5. If you did many changes, it might simplify things to [move some of the uncommited changes to a new branch](https://betterstack.com/community/questions/move-uncommited-work-to-new-branch/){:target="_blank"}.

## Security considerations

You should not store sensitive information in any remote repository, including GitHub, GitLab, etc. In fact, you should avoid sensitive information in your research files at all times. This includes PHI, passwords, etc. Remember, most of the data you upload to GitHub will be publicly accessible. It is very easy to accidentally leave a password or sensitive file in your code, which you then upload.

Also consider the code you might clone from other users on GitHub. Use caution to avoid downloading malicious software.

## Additional Info

Git and GitHub are very well-documented and popular tools. There are many great tutorials to make using git easier. We encourage you to seek out more tutorials as needed.

There are also many software tools to help work with Git and GitHub. These include [GitHub Desktop](https://desktop.github.com/){:target="_blank"} and [VS Code](https://code.visualstudio.com/){:target="_blank"}, among many others.
