# Permission Changes

!!! warning "Proceed with caution"
    When modifying file permissions in Linux, use caution and take time to fully understand the effect of your proposed permission changes. Errant permission changes can make a file unusable by yourself, or others.

Users may want to modify file/directory permissions to enable sharing. This should only be done in spaces that are meant for sharing. Each lab has shared group and scratch directories with inherited group permissions. **Never modify permissions to open your home directory for sharing**.

If you need group write permission for a file (not recommended in Linux):

```bash
chmod g+w /path/to/file
```

Here we add the `w` write permission, for `g` group.

!!! tip "Why is group write not recommended?"
    Linux does not include group write permission by default, even in your shared lab spaces. This protects users. For instance, if multiple users attempt to edit the same file, collisions may occur and there is a high risk the file would be corrupted and information lost. Again, group write permissions are not recommended. It is much better for each user to make their own copy of a file.

If you'd like to add write permissions on a directory and its files, add the `-R` recursive flag:

```bash
chmod -R g+w /group/PI_NetID/work/path/to/directory
```

You can check permissions before and after with command `ls -l /group/PI_NetID/work/path/to/file`.

## Advanced

Additional utilities are available to help you manage your file permissions. Here we'll highlight the `find` utility for locating and managing file objects.

To add group write permissions on all files in the current directory:

```bash
find . -type f -exec chmod g+w {} \;
```

To add group write permissions on all directories in the current directory:

```bash
find . -type d -exec chmod g+w {} \;
```
