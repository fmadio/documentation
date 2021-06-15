# Mounting Host Scratch Disk

Mounting the host systems file system into the LXC container can be very helpful in many situations.

Easiest way is via the lxc config file example shows mounting the host /mnt/store1 directory directly into the container. Please ensure rootfs/mnt/store1 directory has been created within the container.

```text
lxc.mount.entry = /mnt/store1 mnt/store1 none bind 0 0

```

NOTE that it is critical to have no leading "/" in the container mount point \(making it a relative mount point\)

[https://wiki.debian.org/LXC\#Bind\_mounts\_inside\_the\_container](https://wiki.debian.org/LXC#Bind_mounts_inside_the_container)

