<!-- TITLE: Lvm Recover Volume That Was Not Exported -->

It seems that if you re-create the same exact PV , VG , LV , on the lost LVM configuration , and then use this command :


```sh
> e2fsck -fp /dev/VolumeGroup/lv_home
```

and then mount the fixes-repaired lvm with:


```sh
> mount -t ext4 /dev/VolumeGroup/lv_home /mountdir
```

it will bring the data back !

anyways this is an un-confirmed and experimental solution and you may not be as lucky as me . always have backup of your data . and also have backup of /etc/lvm LVM configuration prior to facing a problem like this.

in my case i had a 3 TB HDD , it had one 1 tb LVM and a 2 TB Linux LVM drives as sdb1 and sdb2

i didn't have the saved configuration of the LVM and pvscan couldn't find the PV at all.pvs -vvvv could show the drives.

i this the following commands and the LVM came back


```sh
> pvcreate /dev/sdb1
> pvcreate /dev/sb2

> vgcreate vg1 /dev/sdb1
> vgextend vg1 /dev/sdb2

> lvcreate --name lv1 -l 100%FREE vg1
```

then pvs , lvs , vgs displayed this new LVM configuration.

then


```sh
> e2fsck -fp /dev/vg1/lv1
```

and it fixed lots of errors.

then


```sh
> mount -t ext4 /dev/vg1/lv1 /home2
```

and here it is my customer data is back


```sh
> df -h /dev/mapper/vg1-lv1 2.7T 266G 2.4T 10% /home2
```




