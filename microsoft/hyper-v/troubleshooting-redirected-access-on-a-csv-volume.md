<!-- TITLE: Troubleshooting Redirected Access On A Csv Volume -->

Cluster shared Volumes (CSV) is a new feature implemented in Windows Server 2008 R2 to assist with new scale-up\out scenarios. CSV provides a scalable fault tolerant solution for clustered applications that require NTFS file system access from anywhere in the cluster. In Windows Server 2008 R2, CSV is only supported for use by the Hyper-V role.

The purpose of this blog is to provide some basic troubleshooting steps that can be executed to address CSV volumes that show a **Redirected Access** status in Failover Cluster Manager. It is not my intention to cover the Cluster Shared Volumes feature. For more information on Cluster Shared Volumes consult TechNet.

Before diving into some troubleshooting techniques that can be used to resolve **Redirected Access** issues on Cluster Shared Volumes, let’s list some of the basic requirements for CSV as this may help resolve other issues not specifically related to **Redirected Access**.

Disks that will be used in the CSV namespace must be MBR or GPT with an NTFS partition.
* The drive letter for the system disk must be the same on all nodes in the cluster.
* The NTLM protocol must be enabled on all nodes in the cluster.
* Only the in-box cluster “Physical Disk” resource type can be added to the CSV namespace. No third party storage resource types are supported.
* Pass-through disk configurations cannot be used in the CSV namespace.
* All networks enabled for cluster communications must have Client for Microsoft Networks and File and Printer Sharing for Microsoft Networks protocols enabled.
* All nodes in the cluster must share the same IP subnets between them as CSV network traffic cannot be routed. For multi-site clusters, this means stretched VLANs must be used. 

Let’s start off by looking at the CSV namespace in a Failover Cluster when all things appear to be ‘normal.’ In Figure 1, all CSV volumes show **Online** in the Failover Cluster Management interface.

![6886 Clip Image 002 69 F 3 D 528](/uploads/6886-clip-image-002-69-f-3-d-528.jpg "6886 Clip Image 002 69 F 3 D 528")

Looking at a CSV volume from the perspective of a highly available Virtual Machine group (Figure 2), the Virtual Machine is **Online** on one node of the cluster (R2-NODE1), while the CSV volume hosting the Virtual Machine files is **Online** on another node (R2-NODE2) thus demonstrating how CSV completely disassociates the Virtual Machine resources (Virtual Machine; Virtual Machine Configuration) from the storage hosting them.

![5633 Clip Image 004 342 E 6 A 01](/uploads/5633-clip-image-004-342-e-6-a-01.jpg "5633 Clip Image 004 342 E 6 A 01")

When all things are working normally (no backups in progress, etc…) in a Failover Cluster with respect to CSV, the vast majority of all storage I/O is Direct I/O meaning each node hosting a virtual machine(s) is writing directly (via Fibre Channel, iSCSI, or SAS connectivity) to the CSV volume supporting the files associated with the virtual machine(s).  A CSV volume showing a **Redirected Access** status indicates that all I/O to that volume, from the perspective of a particular node in the cluster, is being redirected over the CSV network to another node in the cluster which still has direct access to the storage supporting the CSV volume.  This is, for all intents and purposes, a ‘recovery’ mode.  This functionality prevents the loss of all connectivity to storage.  Instead, all storage related I/O is redirected over the CSV network.  This is very powerful technology as it prevents a total loss of connectivity thereby allowing virtual machine workloads to continue functioning.  This provides the cluster administrator an opportunity to evaluate the situation and live migrate workloads to other nodes in the cluster not experiencing connectivity issues. All this happens behind the scenes without users knowing what is going on.  The end result may be slower performance (depending on the speed of the network interconnect, for example, 10 GB vs. I GB) since we are no longer using direct, local, block level access to storage.  We are, instead, using remote file system access via the network using SMB.

There are basically four reasons a CSV volume may be in a **Redirected Access** mode. 

* The user intentionally places the CSV Volume in Redirected Access mode.
* There is a storage connectivity failure for a node in which case all I\O is redirected over a cluster network designated for CSV traffic to another node.
* A backup of a CSV volume is in progress or failed.
* An incompatible filter driver is installed on the node.

Lets’ take a look at a CSV volume in Redirected Access mode (Figure 3).

![7041 Clip Image 006 1 D 1 Fe 8 C 5](/uploads/7041-clip-image-006-1-d-1-fe-8-c-5.jpg "7041 Clip Image 006 1 D 1 Fe 8 C 5")

When a CSV volume is placed in **Redirected Access** mode, a Warning message (Event ID 5136) is registered in the System Event log.

![4743 Clip Image 008 61 Ec 0 Cf 9](/uploads/4743-clip-image-008-61-ec-0-cf-9.jpg "4743 Clip Image 008 61 Ec 0 Cf 9")

Let’s look at each one of the four reasons I mentioned and propose some troubleshooting steps that can help resolve the issue.

1. **User intentionally places a CSV volume in Redirected Access mode:**  Users are able to manually place a CSV volume in **Redirected Access** mode by simply selecting a CSV volume, **Right-Click** on the resource, select **More Actions** and then select **Turn on redirected access for this Cluster shared volume**.

![0028 Clip Image 010 4 D 1 A 1479](/uploads/exchange/0028-clip-image-010-4-d-1-a-1479.jpg "0028 Clip Image 010 4 D 1 A 1479")
Figure 5
Therefore, the first troubleshooting step should be to try turning off Redirected Access mode in the Failover Cluster Management interface.
