<!-- TITLE: Live Resize Lvm Partition -->
<!-- SUBTITLE: A quick summary of Live Resize Lvm Partition -->

Reference : https://access.redhat.com/solutions/199573

##  In order to resize online a partition which is in use please observe the following steps:

```text
# fdisk -l /dev/vda

Disk /dev/vda: 32.2 GB, 32212254720 bytes, 62914560 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000db7e6

   Device Boot      Start         End      Blocks   Id  System
/dev/vda1   *        2048     1026047      512000   83  Linux
/dev/vda2         1026048    28289023    13631488   8e  Linux LVM

# cat /proc/partitions 
major minor  #blocks  name

 252        0   31457280 vda
 252        1     512000 vda1
 252        2   13631488 vda2
  11        0    1048575 sr0
 253        0   10240000 dm-0
 253        1    2129920 dm-1

# pvs
  PV         VG          Fmt  Attr PSize  PFree
  /dev/vda2  rhel_vm-205 lvm2 a--  13.00g 1.20g
```

## Modify the on-disk partition table as usual (e.g. by using fdisk command).
### Delete the partition:

```text
Command (m for help): d
Partition number (1,2, default 2): 2
Partition 2 is deleted
```

### Re-create the partition with the new size:

```text
Command (m for help): n
Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): p
Partition number (2-4, default 2): 2
First sector (1026048-62914559, default 1026048): 
Using default value 1026048
Last sector, +sectors or +size{K,M,G} (1026048-62914559, default 62914559): +18G
Partition 2 of type Linux and of size 18 GiB is set

Command (m for help): t
Partition number (1,2, default 2): 2
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'

Command (m for help): p

Disk /dev/vda: 32.2 GB, 32212254720 bytes, 62914560 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000db7e6

   Device Boot      Start         End      Blocks   Id  System
/dev/vda1   *        2048     1026047      512000   83  Linux
/dev/vda2         1026048    38774783    18874368   8e  Linux LVM
```

### Commit changes to on-disk partition table:

```text
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.
```

### While on-disk partition table has been updated, observe that on-memory kernel partition table has not:

```text
# partprobe 
Error: Partition(s) 2 on /dev/vda have been written, but we have been unable to inform the kernel of the change, probably because it/they are in use.  As a result, the old partition(s) will remain in use.  You should reboot now before making further changes.

# cat /proc/partitions | grep vd
 252        0   31457280 vda
 252        1     512000 vda1
 252        2   13631488 vda2
```

## Execute partx (provided by util-linux package) with --update option on the block device to update the in-memory kernel partition table from the on-disk partition table:

```text
# partx -u /dev/vda
```

## Verify that in-memory kernel partition table has been updated with the new size:

```text
# cat /proc/partitions | grep vd
 252        0   31457280 vda
 252        1     512000 vda1
 252        2   18874368 vda2
```

## Proceed with any further steps, in this example by extending the PV on the partition:


```text
# pvresize /dev/vda2
  Physical volume "/dev/vda2" changed
  1 physical volume(s) resized / 0 physical volume(s) not resized

# pvs
  PV         VG          Fmt  Attr PSize  PFree
  /dev/vda2  rhel_vm-205 lvm2 a--  18.00g 6.20g
	
# lvresize /dev/mapper/vg_os-lv_root -l +100%FREE
```







