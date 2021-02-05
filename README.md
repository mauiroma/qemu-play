# QEMU
This repo contains instructions to create a Virtual Machine based of Fedora with qemu

# Requirements
- Download qcow2 image from [Fedora](https://alt.fedoraproject.org/cloud/) or choose image from [openstack](https://docs.openstack.org/image-guide/obtain-images.html)
- Install [qemu](https://www.qemu.org)
- Install hdiutil

# Create Custom Disk
```
qemu-img create -f qcow2 -b Fedora-Cloud-Base-33-1.6.x86_64.qcow2 fedora33.qcow2 20G
```

# Create Cloud Init disk
Create a volume with configurations used when image will be booted
Check [user-data](config/user-data) file
```
hdiutil makehybrid -o init.iso -hfs -joliet -iso -default-volume-name cidata config/
```

# Boot Virtual Machine
```
qemu-system-x86_64 \
-m 2048 \
-vga virtio \
-accel hvf \
-usb \
-device usb-tablet \
-cdrom init.iso \
-k it \
-device e1000,netdev=net0 \
-netdev user,id=net0,hostfwd=tcp::10022-:22 \
-drive file=./fedora33.qcow2,if=virtio 
```

# Access to Virtual Machine
```ssh -p 10022 cloud@localhost```
