---
title: "Expanding Partition on Linux"
date: 2023-02-02
draft: false

tags:
- expanding
- harddrive
---


<!-- ![linux](https://logopond.com/logos/764befce2161b53b5895108e1e8597d7.png) -->
![linux2](https://cdn.dribbble.com/users/2965683/screenshots/7161445/media/0800209a26fcb568edd57dce98b43c71.jpg)

Expanding a partition on Linux (Virtualization) typically involves a few steps. Here's a guide on how to do it using the parted utility. Before proceeding, make sure you have backups of important data, as resizing partitions carries some risk.


## For LVM
1. **Rescan Disk without restart machine**
    
    ```bash
    sudo echo 1>/sys/class/block/sda/device/rescan
    ```

2. **Check Disk space after expanding partition on same disk**

    ```bash
    sudo fdisk -l #used for list all disk
    ```
    if there are red text on the list, just type `w` for rewrite block, there are no affected to file inside partition.
3. **Use parted to Resize Partitions**
    
    - **Check Partition Layout** 
        
        ```bash
        sudo parted /dev/sdX print
        ```
        
        Replace ***/dev/sdX*** with the appropriate device name for your disk (for example, ***/dev/sda***).

 
    
    - **Resize the Partition**
    
         ```bash
        sudo parted resizepart X 100% 
        ```
        this command resizes partition **X** (we can just change **X** with partition number ex: 1 or 2 etc) to use 100% of the available space.

4. **Check   Physical Volume Status (PVS) on LVM**

    ```bash
        sudo pvs
    ```
5. **Resize the Physical Volume**
    
    ```bash
        sudo pvresize /dev/sdXY

        sudo pvdisplay /dev/sdXY
    ```
    this first command used for resize the physical volume and the second one used for show PV data size
    
    Replace ***/dev/sdXY*** with the appropriate device name for your disk (for example, ***/dev/sda3***).

6. **Check Logical Volume Status (LVS)**

    ```bash
        sudo lvs
    ```
7. **Expand the Logical Volume**
    ```bash
        sudo  lvresize -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
    ```

    Replace ***/dev/mapper/ubuntu--vg-ubuntu--lv*** with your LVM name.

8. **Resize the the volume**
    ```bash
        sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
    ```

    Replace ***/dev/mapper/ubuntu--vg-ubuntu--lv*** with your LVM name.

## For Dedicated Mounted Drive
1. **Rescan Disk without restart machine**
    
    ```bash
    sudo echo 1>/sys/class/block/sda/device/rescan
    ```

2. **Check Disk space after expanding partition on same disk**

    ```bash
    sudo fdisk -l #used for list all disk
    ```
    if there are red text on the list, just type `w` for rewrite block, there are no affected to file inside partition.
3. **Use parted to Resize Partitions**
    
    - **Check Partition Layout** 
        
        ```bash
        sudo parted /dev/sdX print
        ```
        
        Replace ***/dev/sdX*** with the appropriate device name for your disk (for example, ***/dev/sda***).

 
    
    - **Resize the Partition**
    
         ```bash
        sudo parted resizepart X 100% 
        ```
        this command resizes partition **X** (we can just change **X** with partition number ex: 1 or 2 etc) to use 100% of the available space.
4. **Resize the the volume**
    ```bash
        sudo resize2fs /dev/sdXY
    ```

    Replace ***/dev/sdXY*** with your LVM name.