# File Cleanup & Archiving

There are several tools and commands available to help you manage your data, including finding large directories that you can compress down or delete if no longer needed.  Keeping your directories free of old or unwanted files will help keep your account under your disk usage quota.

## Check Storage Quota Limits

You can easily find your available storage directories and current utilization on the clusters with the `mydisks` command.

```bash
$ mydisks
=====My Lab=====
Size  Used Avail Use% File
47G   29G   19G  61% /home/user
932G  158G  774G  17% /group/pi
4.6T     0  4.6T   0% /scratch/u/user
4.6T     0  4.6T   0% /scratch/g/pi 
```

## Finding Large Files and Directories

The following commands are available from the Linux command-line on all clusteres.

### du

du is a tool to estimate file space usage.
To list the top 20 files/directories, sorted by size:

```bash
du -ah . | sort -n -r | head -n 20
```

Another variation of the du command to show directories and their sizes:

```bash
du -h --max-depth=1
```

## Compressing And Archiving Directories

So now you have identified some folders that you don't immediately need.  You can compress and archive those directories or files using `tar` and `gzip`.

### tar

Use the following command to compress an entire directory while in your SSH terminal.  It'll recursively compress every file and directory inside the path you specify.

```bash
tar -czvf name-of-archive.tar.gz /path/to/directory-or-file
```

Here are what the switches mean:

```txt
-c: Create an archive.
-z: Compress the archive with gzip.
-v: Display progress in the terminal while creating the archive, also known as “verbose” mode. The v is always optional in these commands, but it’s helpful.
-f: Allows you to specify the filename of the archive.
```

If you have a directory called **myproject** in your current home directory that you want to compress and archive, you can run the following.

```bash
tar -czvf myproject.tar.gz myproject
```

You can also use the tar command to check the contents of your archive. The following will print a list of directories and files in the archive.

```bash
tar -tf myproject.tar.gz
```

Once you are satisfied with your archive, you can then delete the original directory using `rm`.

```bash
rm -rf myproject
```

!!! danger "USE CAUTION!"
    Most deletions in Linux are permanent.

If sometime down the road, you need to access the content of directory **myproject** again, you can extract the archive by running this command.

```bash
tar -xzvf myproject.tar.gz
```

The `-x` flags tells tar to extract.  Once the command completes, you will now have the folder **myproject** available in your current directory.

### Creating a manifest/list of files in an archive

To list files in an .tar.gz achive:

```bash
tar -tf myarchive.tar.gz
```

To create a manifest file of the listing, run the same command, but pipe it to a text file:

```bash
tar -tf myarchive.tar.gz > myarchive.manifest
```

### Using zip to split archives into smaller chunks

Sometimes may want to limit the max size the output archive file, for example to split it up into many smaller archives to make uploading to a share site easier.  In this example, we'll use a zip command to split a 150GB directory into smaller 50GB archive files.

To create a split zip archive of a folder called **my150GBproject** with the max size of each zip file being 50GB:

```bash
zip -r -s 50g my150GBproject.zip my150GBproject/
```

To unzip this archive, you have to do two steps.  The first will combine the parts into a single zip file.  The second will unzip it and create your original directory back.

```bash
unzip -s 0 my150GBproject.zip --out unsplit.zip   # this combines the split files back into one zip
unzip unsplit.zip   # this will exact the files
```

### gzip

You can compress individual files by running `gzip` on the original file.  For example

```bash
gzip myfile.txt
```

This creates a new compressed file called **myfile.txt.gz** in your current directory.

If you later need to access that file again, you can decompress it using this command.

```bash
gunzip myfile.txt.gz
```

You now have the original **myfile.txt** available.

--8<-- "includes/abbreviations.md"
