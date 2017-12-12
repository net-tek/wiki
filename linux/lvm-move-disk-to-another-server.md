<!-- TITLE: Lvm Move Disk To Another Server -->

# Unmount the file system
First, make sure that no users are accessing files on the active
volume, then unmount it


```sh
> unmount /mnt/design/users
```

# Mark the volume group inactive
Marking the volume group inactive removes it from the kernel and prevents any further activity on it.

```sh
> vgchange -an design
vgchange -- volume group "design" successfully deactivated
```

# Export the volume group
It is now necessary to export the volume group. This prevents it from being accessed on the ***old*** host system and prepares it to be removed.


```sh
> vgexport design
vgexport -- volume group "design" successfully exported
```

When the machine is next shut down, the disk can be unplugged and then connected to it's new machine

# Remove orphaned physical disk without reboot
First type the command pvs or pvscan to show on which device you have the error.
Then type lsscsi to show


```sh
> lsscsi
[1:0:0:0]    cd/dvd  NECVMWar VMware IDE CDR10 1.00  /dev/sr0
[2:0:0:0]    disk    VMware   Virtual disk     1.0   /dev/sda
[2:0:1:0]    disk    VMware   Virtual disk     1.0   /dev/sdb <== the removed physical disk
[2:0:2:0]    disk    VMware   Virtual disk     1.0   /dev/sdc
```

Now write the delete file with


```sh
> echo 1 > /sys/class/scsi_device/2\:0\:1\:0/device/delete
```

# Adding the disk to the new machine and scan SCSI bus

```sh
> echo "- - -" > /sys/class/scsi_host/host#/scan
```

# Import the volume group
When plugged into the new system it becomes /dev/sdb so an initial pvscan shows:


```sh
> pvscan
pvscan -- reading all physical volumes (this may take a while...)
pvscan -- inactive PV "/dev/sdb1"  is in EXPORTED VG "design" [996 MB / 996 MB free]
pvscan -- inactive PV "/dev/sdb2"  is in EXPORTED VG "design" [996 MB / 244 MB free]
pvscan -- total: 2 [1.95 GB] / in use: 2 [1.95 GB] / in no VG: 0 [0]
```

We can now import the volume group (which also activates it) and mount the file system. If you are importing on an LVM 2 system, run:


```sh
> vgimport design
Volume group "vg" successfully imported
```

If you are importing on an LVM 1 system, add the PVs that need to be imported:


```sh
> vgimport design /dev/sdb1 /dev/sdb2
vgimport -- doing automatic backup of volume group "design"
vgimport -- volume group "design" successfully imported and activated
```

# Activate the volume group
You must activate the volume group before you can access it.


```sh
> vgchange -ay design
```

# Mount the file system

```sh
> mkdir -p /mnt/design/users
> mount /dev/design/users /mnt/design/users
```









