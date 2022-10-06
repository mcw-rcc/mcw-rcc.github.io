# Sharing Data

Your lab group storage can be used to collaborate with non-lab members. This is done with by-request project directories. For example, if lab `pi` would like to collaborate with some data for a project called `zephyr`, then the lab PI would contact RCC to create a project directory.

```bash
$ ls -l /group/pi
total 8
drwxrws---. 3 root sg-pi-zephyr 512 Mar 26 13:52 zephyr
drwxrws---. 4 root sg-pi       1024 May 19 14:04 work
```

In addition to the usual `work` directory, there is now a `zephyr` project directory, which is controlled by the `sg-pi-zephyr` security group. Project data would go in that directory, and any number of collaborators (MCW researchers) can be added to the security group.
