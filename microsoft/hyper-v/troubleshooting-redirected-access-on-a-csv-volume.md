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

