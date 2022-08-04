# Research Group Storage

## Overview
Research Group Storage (RGS) is a storage system designed to hold active research data at any scale. It is organized by PI research group and allows collaborative work with group permissions. 

### Features:

* Every faculty member eligible for 1TB free storage for their lab
* Accessible via Linux or Windows permissions
* 7 days of snapshots protect against accidental deletion
* Collaborative project directories with secure group permissions

### Rates

Every PI research group will receive [1TB of free space](#first-1tb-free) when the PI signs up for an RCC account. If a research group requires more than 1TB, [additional space can be purchased](#paid-additional-storage) with a $60/TB/year subscription.


## Storage Details
### Hardware
RGS is based on Qumulo storage servers. It is designed for long-term storage of data to be processed on the RCC cluster. It is ***not intended for I/O intensive workloads***. The initial total storage capacity is `1.77PB`. The hardware is enterprise-grade and highly resilient, using a 3-disk/1-node failure protection. All hardware is covered by a support contract.

!!! danger "RGS has no disaster recovery (i.e. backup)"
    All users are responsible for the integrity of their own data.

### Data Protection
Additional data protection is provided in the form of snapshots. These are system level, automated incremental on-disk backups. Snapshots help protect against accidental deletions of data. RCC keeps 7 days of snapshots for all RGS data. Snapshots do not protect against catastrophic failure of hardware, and RCC does not maintain a second copy of any data stored on RGS hardware.

!!! danger "RGS has no disaster recovery (i.e. backup)"
    All users are responsible for the integrity of their own data.

### Data Loss
RCC is not responsible for any loss of data stored on RGS. The RGS system is designed to store a single copy of active research data. RCC strongly encourages all users to maintain at least one alternate copy of any data stored in RGS.

!!! danger "RGS has no disaster recovery (i.e. backup)"
    All users are responsible for the integrity of their own data.

### Group Permissions
Every PI storage directory will have an associated LDAP/AD group consisting of the PI and additional RCC users that the PI adds. RCC requires two points of contact that are authorized to request permissions changes. The PI will serve as one point of contact and will provide an alternate. Any group permission changes must come from the PI or alternate. ***Changes must be made through the MCW ticketing system.***

### Data Restrictions
The following types of data are strictly prohibited :

* Any data that, if placed on RGS, would violate the MCW Code of Conduct, MCW Corporate Polices, or any applicable data-use agreement (i.e. IRB, federal grant regulations, etc.)

* Any data that is subject to HIPAA

!!! warning "PI is responsible for proper use of RGS"
    Please contact {{ support_email }} if you have any questions about data restrictions on RGS.

## First 1TB Free
Every MCW PI will receive 1TB of free space on RGS when they sign up for an RCC account. This storage is available on the cluster at `/group/{PI_NetID}` and via NFS/SMB by request.

## Paid Additional Storage
Many MCW labs have large amounts of research data. If a lab requires more than the free 1TB of space, additional space is available with a paid subscription. The additional space is added to the group's free 1TB directory.

### Subscription Cost
The cost is $60/TB/year. All fees must be prepaid. Minimum subscription size is 1TB. Minimum subscription duration is 1 year or the number of months until the start of the next subscription year. RGS subscriptions are based on calendar year (January 1 to December 31). Subscriptions purchased in mid-year will be charged a prorated fee according to the number of months until January 1. RCC will not refund RGS fees for any reason.

#### Example Fee Calculations
:   Purchase a 3TB subscription on June 1, 2021 with minimum duration (i.e. # of mos until Jan 1). The fee will be \$105 (\$5/mo/TB x 7mos x 3TB), with renewal date of Jan 1, 2022. New total quota limit is 4TB.

:   Purchase a 5TB subscription on March 1, 2021 with 3 year duration (i.e. # of mos until Jan 1, plus 3yrs). The fee will be \$1150 (\$5/mo/TB x 10mos duration x 5TB + \$60/yr/TB x 3yr x 5TB), with a renewal date of Jan 1, 2025. New total quota limit is 6TB.

#### Unpaid Fees
If RGS fees are unpaid, RCC admins will change your storage directory to **read-only** (i.e. no new data can be written). The PI will have until March 31 to pay any fees that are due or move the data off of RGS.

### Ownership
The RGS service is a subscription-based lease of storage space for a finite duration. RCC retains ownership of all hardware associated with RGS.

### Availability
RCC will not guarantee availability of storage for RGS subscription. Requests for subscription greater than 50TB will require RCC review.

### Accessibility
All RGS storage directories will be available on the cluster at `/group`. A PI may request additional access with either an NFS or SMB share.

## Sign-up & Payment
### Sign-up
A RCC account is required for RGS. If you do not already have an RCC account, [submit a request](https://forms.office.com/r/98QNm6cAyt). RCC will then process your account and provision the storage. If you are not a PI, your PI must also have an RCC account.

### Payment
RCC is accepting payment starting Dec. 1, 2020. Payment is due by Jan. 1, 2021. Payment can be made through the MCW-IS service desk website using the following guide.

1. Proceed to <https://servicedesk.mcw.edu> and login.
2. Select **RCC Software**, then **Research Group Storage - 1TB**.
3. To pay for 1TB, select **Add to Cart**. If you are paying for multiple TBs, select **Add Multiple** and enter the quantity.
4. Finally, select **Place Your Order** and enter your 16-17 Digit Account Number.

!!! note "Questions?"
    If you have questions about your quota limit, please email {{ support_email }}. If you have questions about the self-service payment process, please contact the MCW-IS help desk.

--8<-- "includes/abbreviations.md"