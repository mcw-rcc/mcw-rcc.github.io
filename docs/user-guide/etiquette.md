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
RCC does enforce usage on the cluster login nodes through a service named Arbiter. This service leverages Linux cgroups in order to monitor cpu and  memory usage.

!!! info "Login node per-user limit is 8 CPU cores and 40 GB memory."
    When a user is at or above the threshold limit, they start to accrue a "badness score". If a user remains at or above the threshold limit, the badness score increases until it results in a "penalty" condition, which further limits the CPU and memory usage accessible to the user. The penalty condition will throttle usage in order to not violate the established usage policy. Arbiter also maintains a history of penalty status, resulting in a time period before previous behavior is not a factor on new penalties. Users will receive an email message when they enter and exit penalty status. The email will include a listing of their high impact processes, and a graph that shows their CPU and memory usage over a period of time.
