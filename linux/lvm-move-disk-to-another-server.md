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
> vgchange -- volume group "design" successfully deactivated
```

# Export the volume group
It is now necessary to export the volume group. This prevents it from being accessed on the ***old*** host system and prepares it to be removed.


```sh
> vgexport design
> vgexport -- volume group "design" successfully exported
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


