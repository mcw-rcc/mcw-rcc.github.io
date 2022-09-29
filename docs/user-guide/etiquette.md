# User Etiquette

RCC clusters are shared resources. Please be respectful of your computational neighbors and adhere to the following guidelines.

## Guidelines

1. All jobs must be run through the queueing system.

    For the cluster to work properly as a shared resource, all jobs must go through the queueing system.

2. Do not start computationally intensive work on cluster login nodes.

    Login nodes are designed for user logins, managing jobs, and accessing data storage. All three of those services must work for the cluster to function. If you start intensive computing on a login node, you may cause some or all of those services to fail or the node itself to fail, resulting in lost work for you and others.

3. User login to compute nodes is prohibited.

    Any processing on a compute node that is done outside the queueing system can cause the node to fail. Simply put, if you’re not supposed to be computing there, you don’t need to be logged in.

4. Be accurate with your resource requests.

    Do not request more resources than are needed to complete your job. Most jobs are not parallel. Requesting multiple cores when your job cannot use them wastes resources that might be used by other users. This is a very common problem! Do your best and ask for help if you're unsure.

## Enforcement

Each login node has a safety mechanism built-in to prevent a user from crashing the server or other users' processes. This per-user limit is **4 CPU cores** and **20 GB memory**. When a user is at or above their limit, the system will throttle their processes. Users will receive email messages when violations occur.

!!! danger "Intensive computing on a login node is prohibited."
    If a command requires more than 1 core, then it should be submitted in a job to the scheduler. The 4 core limit is a safety mechanism to prevent accidentally crashing a login node, not an approval to run a small computational workflow.
