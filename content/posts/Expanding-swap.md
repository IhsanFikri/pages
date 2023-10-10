---
title: "Expanding Swap Space on Linux [en]"
date: 2023-01-26
draft: false

tags:
- swap space
- expanding
---
![linux2](https://cdn.dribbble.com/users/2965683/screenshots/7161445/media/0800209a26fcb568edd57dce98b43c71.jpg)


&nbsp;&nbsp;&nbsp;On a Linux system, swap space is used as a virtual memory extension when the system's physical RAM (Random Access Memory) is fully utilized. When your computer runs out of physical memory because of the applications and processes you're running, the operating system moves data from the RAM to an area of the hard drive or SSD called swap space

1.  **Check the Current Swap Space**

    `free -m` or `swapon -s` or `htop`

2. **Create Swap File of size 1GB**

    `# dd if=/dev/zero of=/swap_file bs=1G count=1`
    
    Replace the value of **‘bs‘** and **‘count‘** according your requirement.
    We can also use fallocate command to create a file, example is show below.
        
    `#fallocate -l 1G /swap_file`
    
3. **Secure the swap file**

    `# chmod 600 /swap_file`

4. **Enable the Swap Area on Swap File**

    `mkswap /swap_file`

5. **Add the swap file entry in fstab file**

    `echo "swap_file  swap   swap   defaults   0 0" >> /etc/fstab`

6. **Extend Swap Space**

    `swapon /swap_file`

7. **Now verify the swap space**

    `free -m` or `swapon -s` or `htop`