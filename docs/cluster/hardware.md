# Overview

The HPC environment became available to MCW researchers in March 2021. The cluster consists of **79** compute nodes, **4,200** CPU cores, and **96** GPUs. The cluster is connected by **7** 100 Gbps switches running RoCEv2 (ethernet equivalent to Infiniband). Additionally, a **780 TB** NVMe array provides scratch storage, and a **2.6 PB** scale-out NAS provides persistent storage.

[Cluster Status](https://slurmdash.rcc.mcw.edu){ .md-button .md-button--primary target="_blank" rel="noopener" }

## Cluster

Detailed information is available below. Please note, the table is wide and might require side scrolling to view all data.

{{ pd_read_csv("includes/cluster-hardware.csv", skip_blank_lines=True, keep_default_na=False) | convert_to_md_table }}

!!! tip "Condo hardware"
    Condo nodes are factored into the overall cluster metrics, but specific hardware details for condo systems are not listed in the table.
