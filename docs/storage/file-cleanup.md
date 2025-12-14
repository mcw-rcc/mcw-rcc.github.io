# Cleanup & Archiving

There are several tools and commands available to help you manage your data, including finding large directories that you can compress down or delete if no longer needed.  Keeping your directories free of old or unwanted files will help keep your account under your disk usage quota.

## Check Storage Quota Limits

You can easily find your available storage directories and current utilization on the clusters with the `mydisks` command.

```txt
$ mydisks
=====My Lab=====
Size  Used Avail Use% File
47G   29G   19G  61% /home/user
932G  158G  774G  17% /group/pi
4.6T     0  4.6T   0% /scratch/g/pi 
```

## Finding Large Files and Directories

Several tools exist to help identify file size and type. Here we discuss use of `du`.

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

To recursively compress every file and directory inside the path you specify:

```bash
tar -czvf name-of-archive.tar.gz /path/to/directory-or-file
```

The `tar` command has many switches. We used the most common set in the previous command:

```txt
-c Create an archive.  
-z Compress the archive with gzip.  
-v Display progress in the terminal while creating the archive, also known as “verbose” mode.  
-f Allows you to specify the filename of the archive.
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

### Creating a file manifest

When archiving a dataset, it is often helpful to have a full manifest, or list, of the original file hierarchy. A file manifest is also a key piece of metadata.

To list files in an .tar.gz achive:

```bash
tar -tf myarchive.tar.gz
```

To create a manifest file of the listing, run the same command, but pipe it to a text file:

```bash
tar -tf myarchive.tar.gz > myarchive.manifest
```

### Managing archive file size

Some file archives can be quite large. If you're uploading your archive to a repository, there may be a max single file upload size. In this case it is useful to split the archive file into smaller chunks.

To split a `.tar.gz` archive into smaller chunksize:

```bash
split -b 500M myarchive.tar.gz "myarchive.tar.gz.part"
```

Now you should have a set of 500M files.

To join the file chunks and recreate the full archive:

```bash
cat myarchive.tar.gz.parta* > myarchive.tar.gz.joined
```

--8<-- "includes/abbreviations.md"
