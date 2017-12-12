<!-- TITLE: Lvm Move Disk To Another Server -->

# Unmount the file system
First, make sure that no users are accessing files on the active
volume, then unmount it


```sh
# unmount /mnt/design/users
```

# Mark the volume group inactive
Marking the volume group inactive removes it from the kernel and prevents any further activity on it.

```sh
# vgchange -an design
# vgchange -- volume group "design" successfully deactivated
```
