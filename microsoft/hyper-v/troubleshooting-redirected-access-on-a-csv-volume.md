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
Figure 1

Looking at a CSV volume from the perspective of a highly available Virtual Machine group (Figure 2), the Virtual Machine is **Online** on one node of the cluster (R2-NODE1), while the CSV volume hosting the Virtual Machine files is **Online** on another node (R2-NODE2) thus demonstrating how CSV completely disassociates the Virtual Machine resources (Virtual Machine; Virtual Machine Configuration) from the storage hosting them.

![5633 Clip Image 004 342 E 6 A 01](/uploads/5633-clip-image-004-342-e-6-a-01.jpg "5633 Clip Image 004 342 E 6 A 01")
Figure 2

When all things are working normally (no backups in progress, etc…) in a Failover Cluster with respect to CSV, the vast majority of all storage I/O is Direct I/O meaning each node hosting a virtual machine(s) is writing directly (via Fibre Channel, iSCSI, or SAS connectivity) to the CSV volume supporting the files associated with the virtual machine(s).  A CSV volume showing a **Redirected Access** status indicates that all I/O to that volume, from the perspective of a particular node in the cluster, is being redirected over the CSV network to another node in the cluster which still has direct access to the storage supporting the CSV volume.  This is, for all intents and purposes, a ‘recovery’ mode.  This functionality prevents the loss of all connectivity to storage.  Instead, all storage related I/O is redirected over the CSV network.  This is very powerful technology as it prevents a total loss of connectivity thereby allowing virtual machine workloads to continue functioning.  This provides the cluster administrator an opportunity to evaluate the situation and live migrate workloads to other nodes in the cluster not experiencing connectivity issues. All this happens behind the scenes without users knowing what is going on.  The end result may be slower performance (depending on the speed of the network interconnect, for example, 10 GB vs. I GB) since we are no longer using direct, local, block level access to storage.  We are, instead, using remote file system access via the network using SMB.

There are basically four reasons a CSV volume may be in a **Redirected Access** mode. 

* The user intentionally places the CSV Volume in Redirected Access mode.
* There is a storage connectivity failure for a node in which case all I\O is redirected over a cluster network designated for CSV traffic to another node.
* A backup of a CSV volume is in progress or failed.
* An incompatible filter driver is installed on the node.

Lets’ take a look at a CSV volume in Redirected Access mode (Figure 3).

![7041 Clip Image 006 1 D 1 Fe 8 C 5](/uploads/7041-clip-image-006-1-d-1-fe-8-c-5.jpg "7041 Clip Image 006 1 D 1 Fe 8 C 5")
Figure 3

When a CSV volume is placed in **Redirected Access** mode, a Warning message (Event ID 5136) is registered in the System Event log. (Figure 4).

![4743 Clip Image 008 61 Ec 0 Cf 9](/uploads/4743-clip-image-008-61-ec-0-cf-9.jpg "4743 Clip Image 008 61 Ec 0 Cf 9")
Figure 4

Let’s look at each one of the four reasons I mentioned and propose some troubleshooting steps that can help resolve the issue.

1. **User intentionally places a CSV volume in Redirected Access mode:**  Users are able to manually place a CSV volume in **Redirected Access** mode by simply selecting a CSV volume, **Right-Click** on the resource, select **More Actions** and then select **Turn on redirected access for this Cluster shared volume**. (Figure 5).

![0028 Clip Image 010 4 D 1 A 1479](/uploads/exchange/0028-clip-image-010-4-d-1-a-1479.jpg "0028 Clip Image 010 4 D 1 A 1479")
Figure 5
Therefore, the first troubleshooting step should be to try turning off Redirected Access mode in the Failover Cluster Management interface.

2. **There is a storage connectivity issue:**  When a node loses connectivity to attached storage that is supporting a CSV volume, the cluster implements a recovery mode by redirecting storage I\O to another node in the cluster over a network that CSV can use.  The status of the cluster Physical Disk resource associated with the CSV volume is **Redirected Access** and all storage I\O for the associated virtual machine(s) being hosted on that volume is redirected over the network to another node in the cluster that has direct access to the CSV volume.  This is by far the `number one reason` CSV volumes are placed in **Redirected Access** mode. Troubleshoot this as you would any other loss of storage connectivity on a server.  Involve the storage vendor as needed.  Since this is a cluster, the cluster validation process can also be used as part of the troubleshooting process to test storage connectivity.

Look for the following event ID in the system event log.


```powershell
Log Name: System
Source: Microsoft-Windows-FailoverClustering
Date: 10/8/2010 6:16:39 PM
Event ID: 5121
Task Category: Cluster Shared Volume
Level: Error
Keywords:
User: SYSTEM
Computer: Node1.cluster.com

Description:Cluster Shared Volume ‘DATA-LUN1’ (‘DATA-LUN1’) is no longer directly accessible from this cluster node. I/O access will be redirected to the storage device over the network through the node that owns the volume. This may result in degraded performance. If redirected access is turned on for this volume, please turn it off. If redirected access is turned off, please troubleshoot this node’s connectivity to the storage device and I/O will resume to a healthy state once connectivity to the storage device is reestablished.
```

3. **A backup of a CSV volume fails:**  When a backup is initiated on a CSV volume, the volume is placed in **Redirected Access** mode.  The type of backup being executed determines how long a CSV volume stays in redirected mode. If a software backup is being executed, the CSV volume remains in redirected mode until the backup completes.  If hardware snapshots are being used as part of the backup process, the amount of time a CSV volume stays in redirected mode will be very short.  For a backup scenario, the CSV volume status is slightly modified.  The status actually shows as **Backup in progress, Redirected Access**  (Figure 6) to allow you to better understand why the volume was placed in **Redirected Access** mode. When the backup application completes the backup of the volume, the cluster must be properly notified so the volume can be brought out of redirected mode.

![7384 Clip Image 012 4 A 90 E 2 Bb](/uploads/exchange/7384-clip-image-012-4-a-90-e-2-bb.jpg "7384 Clip Image 012 4 A 90 E 2 Bb")
Figure 6

A couple of things can happen here.  Before proceeding down this road, ensure a backup is really not in progress. The first thing that needs to be considered is that the backup completes but the application did not properly notify the cluster that it completed so the volume can be brought out of redirected mode.  The proper call that needs to be made by the backup application is ClusterClearBackupStateForSharedVolume which is documented on MSDN.  If that is the case, you should be able to clear the **Backup in progress, Redirected Access** status by simulating a failure on the CSV volume using the cluster PowerShell cmdlet Test-ClusterResourceFailure.  Using the CSV volume shown in Figure 6, an example would be –

`Test-ClusterResourceFailure “35 GB Disk”`

If this clears the redirected status, then the backup application vendor needs to be notified so they can fix their application.

The second consideration concerns a backup that fails, but the application did not properly notify the cluster of the failure so the cluster still thinks the backup is in progress. If a backup fails, and the failure occurs before a snapshot of the volume being backed up is created, then the status of the CSV volume should be reset by itself after a 30 minute time delay.  If, however, during the backup, a software snapshot was actually created (assuming the application creates software snapshots as part of the backup process), then we need to use a slightly different approach.

To determine if any volume shadow copies exist on a CSV volume, use the vssadmin command line utility and run vssadmin list shadows (Figure 7).

![0042 Clip Image 014 7 Ad 7 B 771](/uploads/exchange/0042-clip-image-014-7-ad-7-b-771.jpg "0042 Clip Image 014 7 Ad 7 B 771")
Figure 7

Figure 7 shows there is a shadow copy that exists on the CSV volume that is in **Redirected Access** mode. Use the vssadmin utility to delete the shadow copy (Figure 8).  Once that completes, the CSV volume should come **Online** normally.  If not, change the Coordinator node by moving the volume to another node in the cluster and verify the volume comes **Online**.

![7120 Clip Image 016 37 B 65 C 5 F](/uploads/exchange/7120-clip-image-016-37-b-65-c-5-f.jpg "7120 Clip Image 016 37 B 65 C 5 F")
Figure 8

4. **An incompatible filter driver is installed in the cluster:**  The last item in the list has to do with filter drivers introduced by third party application(s) that may be running on a cluster node and are incompatible with CSV.  When these filter drivers are detected by the cluster, the CSV volume is placed in redirected mode to help prevent potential data corruption on a CSV volume.  When this occurs an Event ID 5125[EC4]  **Warning** message is registered in the System Event Log.  Here is a sample message –


```text
17416 06/23/2010 04:18:12 AM   Warning       <node_name>  5125    Microsoft-Windows-FailoverClusterin Cluster Shared Vol NT AUTHORITY\SYSTEM               Cluster Shared Volume ‘Volume2’ (‘Cluster Disk 6’) has identified one or more active filter drivers on this device stack that could interfere with CSV operations. I/O access will be redirected to the storage device over the network through another Cluster node. This may result in degraded performance. Please contact the filter driver vendor to verify interoperability with Cluster Shared Volumes.  Active filter drivers found: <filter_driver_1>,<filter_driver_2>,<filter_driver_3>
```

The cluster log will record warning messages similar to these –

```text
7c8:088.06/10[06:26:07.394](000000) WARN  [DCM] filter <filter_name> found at unsafe altitude <altitude_numeric> 
7c8:088.06/10[06:26:07.394](000000) WARN  [DCM] filter <filter_name>  found at unsafe altitude <altitude_numeric> 
7c8:088.06/10[06:26:07.394](000000) WARN  [DCM] filter <filter_name>   found at unsafe altitude <altitude_numeric>
```

Event ID 5125 is specific to a file system filter driver.  If, instead, an incompatible volume filter driver were detected, an Event ID 5126 would be registered.  For more information on the difference between file and volume filter drivers, consult MSDN.

**Note:**  Specific filter driver names and altitudes have been intentionally left out.  The information can be decoded by downloading the ‘File System Minifilter Allocated Altitudes’ spreadsheet posted on the Windows Hardware Developer Central public website.

Additionally, the fltmc.exe command line utility can be run to enumerate filter drivers.  An example is shown in Figure 9.

![8054 Clip Image 018 73 A 8552 C](/uploads/exchange/8054-clip-image-018-73-a-8552-c.jpg "8054 Clip Image 018 73 A 8552 C")
Figure 9

Once the Third Party filter driver has been identified, the application should be removed and\or the vendor contacted to report the problem.  Problems involving Third Party filter drivers are rarely seen but still need to be considered.

UPDATE 4/9: A Hotfix has been released to address an issue where filter drivers can cause the ‘redirected access’ issue:

FIXED: Cluster Shared Volumes (CSV) in redirected access mode after installing McAfee VSE 8.7 Patch 5 or 8.8 Patch 1 http://blogs.technet.com/b/askcore/archive/2012/03/18/fixed-cluster-shared-volumes-csv-in-redirected-access-mode-after-installing-mcafee-vse-8-7-patch-5-or-8-8-patch-1.aspx

Hopefully, I have provided information here that will get you started down the right path to resolving issues that involve CSV volumes running in a Redirected Access mode.