---
title: "Restore LVM from Attached disk [id]"
date: 2024-09-19
draft: false

tags:
- bash
---

---

**1. Scan all new disk**
```
for host in /sys/class/scsi_host/*; do echo "- - -" | sudo tee $host/scan; ls /dev/sd* ; done
```

**2. Check attached disk**
```
    fdisk -l
```

**3. heck phisical disk on lvm `pvs`**

| PV |VG | Fmt | Attr | PSize|PFre|
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| /dev/sda3  |ubuntu-vg| lvm2 |a--  |<48.00g  |  0
| /dev/sdb3  |ubuntu-vg |lvm2 |a--  |<99.00g    |0


**4. if vgname is same do this instead**
```
vgimportclone -n ubuntu-vg_clone /dev/sdb3
```

**5. active the vg** 
```
vgchange -ay
```

then check again using `pvs` the vg name will be change like we set before

| PV |VG | Fmt | Attr | PSize|PFre|
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| /dev/sda3  |ubuntu-vg| lvm2 |a--  |<48.00g  |  0
| /dev/sdb3  | ubuntu-vg_clone |lvm2 |a--  |<99.00g    |0

**6. and then we can mount the data from LVM**
```
mount /dev/ubuntu-vg_clone/ubuntu-lv /mnt
```

**7. Do as you wish**

new LVM is mounted at `/mnt`